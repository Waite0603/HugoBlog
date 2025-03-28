---
title: "单例模式以及事件管理器在 Cocos 中的应用"
date: 2025-03-28T07:30:38Z
lastmod: 2025-03-28T07:30:38Z
categories: ["Ai"]
tags: ["Ai", "Python", "Vue"]
author: "Waite Wang"
showToc: true
TocOpen: true
draft: false
hidemeta: false
disableHLJS: true
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

## 后端请求

> 后端封装

```python
from abc import ABC, abstractmethod
from openai import OpenAI
import config


class BaseAIService(ABC):
  """AI服务的抽象基类"""

  @abstractmethod
  def get_stream_completion(self, messages):
    """获取流式响应"""
    pass

  @abstractmethod
  async def process_stream_response(self, response):
    """处理流式响应"""
    pass


  class HuoShanAIService(BaseAIService):
    """火山引擎 AI 服务实现"""

    def __init__(self):
      self.config = config.HUO_SHAN_COMPLETION_CONFIG
      self.client = OpenAI(
        api_key=self.config["api_key"],
        base_url=self.config["base_url"]
      )

      def get_stream_completion(self, messages):
        """获取流式响应"""
        return self.client.chat.completions.create(
          model=self.config["fast_llm"],
          messages=messages,
          temperature=0.3,
          top_p=1,
          max_tokens=8192,
          stream=True
        )

        async def process_stream_response(self, response):
          """处理流式响应"""
          reasoning_content = ""
          answer_content = ""
          is_answering = False

          try:
            for chunk in response:
              try:
                if not chunk.choices or not chunk.choices[0].delta:
                  continue

                  delta = chunk.choices[0].delta

                  # 处理思考过程
                  if hasattr(delta, 'reasoning_content') and delta.reasoning_content:
                    reasoning_text = delta.reasoning_content
                    print(reasoning_text, end='', flush=True)
                    reasoning_content += reasoning_text
                    yield f"__r__{reasoning_text}".encode('utf-8', errors='ignore')

                    # 处理回复内容
                  elif hasattr(delta, 'content') and delta.content:
                    if not is_answering:
                      print("\n" + "=" * 20 + "完整回复" + "=" * 20 + "\n")
                      is_answering = True

                      content_text = delta.content
                      print(content_text, end='', flush=True)
                      answer_content += content_text
                      yield f"__a__{content_text}".encode('utf-8', errors='ignore')

                    except UnicodeEncodeError as e:
                      print(f"Encoding error: {e}")
                      continue
                    except Exception as e:
                      print(f"Error processing chunk: {e}")
                      yield f"__a__处理响应时发生错误: {str(e)}".encode('utf-8', errors='ignore')

                    except Exception as e:
                      print(f"Stream processing error: {e}")
                      yield f"__a__处理流式响应时发生错误: {str(e)}".encode('utf-8', errors='ignore')


                      def get_ai_service(provider="huoshan"):
                        """工厂函数，根据提供商返回对应的 AI 服务实例"""
                        providers = {
                          "huoshan": HuoShanAIService,
                          # 未来可以添加其他提供商
                          # "openai": OpenAIService,
                          # "azure": AzureAIService,
                        }

                        service_class = providers.get(provider)
                        if not service_class:
                          raise ValueError(f"不支持的 AI 提供商: {provider}")

                          return service_class()
```

> 调用并且返回值

```python
@retrieval_bp.route('/llm_result', methods=['POST', 'FETCH'])
def retrieval():
  messages = [{
    {"role": "system", "content":""},
    {"role": "user", "content":""}  
  }]
  # 创建流式响应
  def stream_generator():
    try:
      print("\n" + "=" * 20 + "思考过程" + "=" * 20 + "\n")

      ai_service = get_ai_service("huoshan")
      llm_response = ai_service.get_stream_completion(messages)

      answer_content = ""

      # 使用同步方式处理响应
      loop = asyncio.new_event_loop()
      asyncio.set_event_loop(loop)
      async_gen = ai_service.process_stream_response(llm_response)
      while True:
        try:
          chunk = loop.run_until_complete(async_gen.__anext__())
          if chunk.startswith(b'__a__'):
            answer_content += chunk.decode('utf-8')[5:]
            yield chunk
            # elif chunk.startswith(b'__r__'):
            # reasoning_content = chunk.decode('utf-8')[5:]
            # yield chunk
          except StopAsyncIteration:
            break
            return Response(
              stream_with_context(stream_generator()),
              mimetype='text/event-stream',
              headers={
                'Cache-Control': 'no-cache',
                'Connection': 'keep-alive',
                'X-Accel-Buffering': 'no',
                'Content-Type': 'text/event-stream',
                'Transfer-Encoding': 'chunked',
                'Access-Control-Allow-Origin': '*',
                'Access-Control-Allow-Headers': '*'
              }  
            )

```

## 前端接收

```javascript
// 流式响应API
export const getStreamLLMResultApi = (data, requestId) => {
  return apiClient.makeRequest('POST', '/retrieval/llm_result', {
    data,
    responseType: 'stream',
    headers: {
      Accept: 'text/event-stream'
    },
    requestId: requestId
  })
}

// 中断所有请求
export const abortAllRequests = () => {
  apiClient.abortAllRequests()
}

// 中断特定请求
export const abortRequest = (requestId) => {
  apiClient.abortRequest(requestId)
}
```

```javascript
const handleStreamResponse = async (data) => {
  try {
    // 生成新的请求ID
    currentRequestId.value = `fortune-${Date.now()}`

    cleanup()
    isTyping.value = true
    displayText.value = ''
    fullText.value = ''
    isThinking.value = true
    // 重置生成的内容
    generatedContent.value = ''

    // 添加延迟以确保过渡效果正常显示
    await new Promise((resolve) => setTimeout(resolve, 100))

    const stream = await getStreamLLMResultApi(data, currentRequestId.value)

    // 如果请求被中断，stream 可能为 null
    if (!stream) return

    const reader = stream.getReader()
    let requestCompleted = false // 标记请求是否完成

    while (true) {
      const { done, value } = await reader.read()
      if (done) {
        requestCompleted = true // 标记请求已完成
        break
      }

      if (value.includes('__r__')) {
        isThinking.value = true
        continue
      }

      isThinking.value = false
      fullText.value += value.replaceAll('__a__', '')
      generatedContent.value += value.replaceAll('__a__', '')
      await typeWriter(value)
    }

    // 只有当请求完成时才保存到缓存
    if (
      requestCompleted &&
      generatedContent.value &&
      generatedContent.value.trim() !== ''
    ) {
      sessionStorage.setItem(
        resolveTitle.value.storageName,
        generatedContent.value
      )
    }
  } catch (error) {
    // 忽略中断错误
    if (error.name !== 'AbortError') {
      console.error('流式请求错误:', error)
    }
    isThinking.value = false
  } finally {
    isTyping.value = false
    renderMarkdown()
  }
}
```

> 前端的渲染

```javascript
import { ref, computed } from 'vue'
import { marked } from 'marked'
import DOMPurify from 'dompurify'
import { debounce } from 'lodash'

export function useMarkdownTyping() {
  const displayText = ref('')
  const renderedContent = ref('')
  const isRendering = ref(false)
  const isTyping = ref(false)
  const fullText = ref('')
  const typingSpeed = 50

  // 创建自定义渲染器
  const renderer = new marked.Renderer()
  renderer.hr = () => ''

  // 配置 marked 选项
  marked.setOptions({
    breaks: true,
    gfm: true,
    headerIds: false,
    mangle: false,
    renderer
  })

  const debouncedTypeWriter = debounce(async (text) => {
    const cleanText = text.replaceAll('__a__', '')
    displayText.value += cleanText

    if (!isRendering.value) {
      renderMarkdown()
    }
  }, 50)

  const debouncedRender = debounce((text) => {
    renderedContent.value = DOMPurify.sanitize(marked(text))
    isRendering.value = false
  }, 100)

  const renderMarkdown = () => {
    isRendering.value = true
    debouncedRender(displayText.value)
  }

  const typeWriter = async (text) => {
    await debouncedTypeWriter(text)
    await new Promise((resolve) => setTimeout(resolve, typingSpeed))
  }

  const cleanup = () => {
    debouncedTypeWriter.cancel()
    debouncedRender.cancel()
  }

  const renderedText = computed(() => renderedContent.value)

  return {
    displayText,
    isTyping,
    fullText,
    renderedText,
    typeWriter,
    cleanup,
    renderMarkdown
  }
}
```