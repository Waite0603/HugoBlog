---
title: "Vue PDF 预览与标注实现解析"
date: 2025-11-11T12:04:23+08:00
categories: ["Vue"]
tags: ["Vue", "Web"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---
# PDF 预览与标注实现解析

最近在做一个文件预览功能，需要在浏览器里展示 PDF 并支持位置标记和缩放。折腾了一段时间，把实现思路整理一下。

## PDF 渲染

PDF 在浏览器里渲染，直接用 `@tato30/vue-pdf` 这个库。它基于 pdf.js，用起来还算顺手。

### 状态定义

先定义几个核心状态：

```typescript
const selectedFile = ref(null)        // 当前选中的文件
const page = ref(1)                    // 当前页码
const isLoading = ref(false)           // 加载状态
const zoomLevel = ref(1)               // 缩放级别，默认 1
const minZoom = 0.5                    // 最小缩放
const maxZoom = 3                      // 最大缩放
const position = ref({ x: 0, y: 0 })   // 拖拽位置
const isDragging = ref(false)          // 是否正在拖拽
const dragStart = ref({ x: 0, y: 0 })  // 拖拽起始位置
```

### PDF URL 计算

PDF 的 URL 用 computed 动态计算，这样文件切换时会自动更新：

```typescript
const currentPdfUrl = computed(() => {
  if (selectedFile.value?.path?.endsWith('.pdf')) {
    return getFileURL('pdf', selectedFile.value.id)
  }
  return ''
})

const { pdf, pages } = usePDF(currentPdfUrl)
const totalPages = computed(() => pages.value || 1)
```

`usePDF` 是个 composable，接收 PDF 的 URL，返回 `pdf` 对象和总页数 `pages`。注意这里用 computed 包装 URL，这样当 `selectedFile` 变化时，PDF 会自动重新加载。

### PDF 监听

监听 PDF 加载完成，更新容器尺寸：

```typescript
watch(pdf, (newPdf) => {
  if (newPdf) {
    isLoading.value = false
    nextTick(updateContainerSize)
  }
})
```

`nextTick` 确保 DOM 更新后再计算尺寸，不然可能拿到旧值。

### 模板渲染

在模板里用 `VuePDF` 组件渲染：

```vue
<VuePDF v-if="pdf" :pdf="pdf" :page="page" :scale="zoomLevel" />
```

传三个参数：`pdf` 对象、当前页码 `page`、缩放比例 `zoomLevel`。组件内部会渲染成 canvas，我们不用管底层细节。

### 文件选择逻辑

选择文件时需要重置视图和页码，并加载标注数据：

```typescript
const selectFile = (file) => {
  resetView()        // 重置缩放和位置
  page.value = 1     // 重置页码
  isLoading.value = true
  selectedFile.value = file
  
  // 处理标注数据，确保页码是数字类型
  if (file.risk_info?.page_info) {
    annotationInfo.value = file.risk_info.page_info.map((item) => ({
      ...item,
      page: typeof item.page === 'string' ? parseInt(item.page) : item.page,
    }))
  } else {
    annotationInfo.value = []
  }
}
```

这里做了类型转换，因为后端可能返回字符串页码，统一转成数字方便后续比较。

## 位置标记的实现

位置标记这块比较有意思。需求是在 PDF 上标出有问题的地方，比如某个区域格式不对、内容有误等等。

### 数据结构

每个标记包含这些信息：
- `page`: 页码
- `location`: 位置坐标，用 `{x1, y1, x2, y2}` 表示，都是 0-1 之间的相对坐标
- `status`: 状态（error、warning、success）
- `reason`: 原因描述

坐标用相对值的好处是，不管 PDF 怎么缩放，标记都能准确对应到位置。

### 叠加层实现

标记不是直接画在 PDF 上的，而是用一个绝对定位的 overlay 层叠在上面。这样不会影响 PDF 本身的渲染。

```vue
<div class="pdf-content">
  <VuePDF v-if="pdf" :pdf="pdf" :page="page" :scale="zoomLevel" />
  <div class="annotations-overlay">
    <div
      v-for="annotation in currentPageAnnotations.filter((a) => a.location)"
      :key="`${annotation.page}-${annotation.location.x}-${annotation.location.y}`"
      class="annotation-marker"
      :class="annotation.status || 'error'"
      :style="getAnnotationStyle(annotation.location)"
    />
  </div>
</div>
```

`annotations-overlay` 用绝对定位铺满整个 PDF 区域，`pointer-events: none` 让它不拦截鼠标事件。但标记本身设置 `pointer-events: auto`，这样鼠标悬停时能触发高亮。

### 位置计算

`getAnnotationStyle` 函数把相对坐标转成 CSS 样式：

```typescript
const getAnnotationStyle = (location) => {
  if (!location) return {}
  return {
    left: `${location.x1 * 100}%`,
    top: `${location.y1 * 100}%`,
    width: `${(location.x2 - location.x1) * 100}%`,
    height: `${(location.y2 - location.y1) * 100}%`,
  }
}
```

比如 `x1: 0.1, x2: 0.3` 就表示从左边 10% 到 30% 的区域。这样无论 PDF 怎么缩放，标记都能跟着走。

### 当前页过滤

只显示当前页的标记，用 computed 过滤：

```typescript
const currentPageAnnotations = computed(() => {
  const currentPage = parseInt(page.value)
  return annotationInfo.value.filter((annotation) => {
    const annotationPage = typeof annotation.page === 'string' 
      ? parseInt(annotation.page) 
      : annotation.page
    return annotationPage === currentPage
  })
})
```

这里做了类型兼容，因为后端可能返回字符串页码。

### 标注列表联动

右侧有个标注列表，点击可以跳转到对应页面并高亮。

#### 状态管理

```typescript
const currentAnnotationIndex = ref(-1)           // 当前标注索引
const highlightedAnnotation = ref(null)          // 高亮的标注
const totalAnnotations = computed(() => annotationInfo.value.length)

// 是否可以切换标注
const canGoPrevAnnotation = computed(() => {
  return totalAnnotations.value > 0
})

const canGoNextAnnotation = computed(() => {
  return totalAnnotations.value > 0
})
```

#### 切换标注逻辑

切换到下一个标注：

```typescript
const nextAnnotation = () => {
  if (canGoNextAnnotation.value) {
    // 循环切换：最后一个切到第一个
    if (currentAnnotationIndex.value >= totalAnnotations.value - 1) {
      currentAnnotationIndex.value = 0
    } else {
      currentAnnotationIndex.value++
    }
    const annotation = annotationInfo.value[currentAnnotationIndex.value]
    
    // 确保页码是数字类型
    if (typeof annotation.page === 'string') {
      annotation.page = parseInt(annotation.page)
    }
    
    // 如果页码不同，切换页面
    if (annotation.page !== page.value) {
      page.value = annotation.page
    }
    highlightAnnotation(annotation)
  }
}
```

切换到上一个标注：

```typescript
const prevAnnotation = () => {
  if (canGoPrevAnnotation.value) {
    // 如果是第一个标注，则切换到最后一个
    if (currentAnnotationIndex.value <= 0) {
      currentAnnotationIndex.value = totalAnnotations.value - 1
    } else {
      currentAnnotationIndex.value--
    }
    const annotation = annotationInfo.value[currentAnnotationIndex.value]
    
    if (typeof annotation.page === 'string') {
      annotation.page = parseInt(annotation.page)
    }
    
    if (annotation.page !== page.value) {
      page.value = annotation.page
    }
    highlightAnnotation(annotation)
  }
}
```

#### 高亮处理

```typescript
const highlightAnnotation = (annotation) => {
  highlightedAnnotation.value = annotation
}

const clearHighlight = () => {
  highlightedAnnotation.value = null
}
```

#### 页面切换自动选择标注

监听页面变化，自动选择当前页面的第一个标注：

```typescript
watch(page, (newPage) => {
  const pageAnnotations = annotationInfo.value.filter((a) => a.page === newPage)
  if (pageAnnotations.length > 0) {
    const index = annotationInfo.value.indexOf(pageAnnotations[0])
    if (index !== -1) {
      currentAnnotationIndex.value = index
      highlightAnnotation(pageAnnotations[0])
    }
  }
})
```

切换标注时会自动跳转到对应页面，如果标注不在当前页的话。页面切换时也会自动选中该页的第一个标注。

## 缩放与拖拽

缩放功能比较常规，但放大后需要支持拖拽移动，不然放大后看不到其他区域就很尴尬。

### 缩放实现

缩放用 CSS `transform: scale()`，配合 `transformOrigin: 'center center'` 让缩放以中心点为基准：

```vue
<div
  class="pdf-content"
  :style="{
    transform: `scale(${zoomLevel}) translate(${position.x}px, ${position.y}px)`,
    transformOrigin: 'center center',
    cursor: isDragging ? 'grabbing' : zoomLevel > 1 ? 'grab' : 'default',
  }"
>
```

`zoomLevel` 范围是 0.5 到 3，每次点击缩放按钮增减 0.1。缩放和位移都放在同一个 `transform` 里，这样性能更好。

### 拖拽逻辑

只有放大（`zoomLevel > 1`）时才允许拖拽，不然没必要。

#### 开始拖拽

```typescript
const startDrag = (event) => {
  if (zoomLevel.value <= 1) return // 只有放大时才能拖动
  isDragging.value = true
  dragStart.value = {
    x: event.clientX - position.value.x,
    y: event.clientY - position.value.y,
  }
}
```

这里有个细节：`dragStart` 存的是鼠标位置减去当前位移，这样拖拽时计算的是相对位移，而不是绝对位置。不然拖拽会"跳"一下。

#### 拖拽中

```typescript
const onDrag = (event) => {
  if (!isDragging.value) return
  event.preventDefault()
  position.value = {
    x: event.clientX - dragStart.value.x,
    y: event.clientY - dragStart.value.y,
  }
}
```

`preventDefault` 防止拖拽时选中文本。

#### 停止拖拽

```typescript
const stopDrag = () => {
  isDragging.value = false
}
```

在 `mouseup` 和 `mouseleave` 事件中都调用，确保鼠标离开元素时也能结束拖拽。

#### 缩放控制

```typescript
const zoomIn = () => {
  if (zoomLevel.value < maxZoom) {
    zoomLevel.value = Math.min(maxZoom, zoomLevel.value + 0.1)
  }
}

const zoomOut = () => {
  if (zoomLevel.value > minZoom) {
    zoomLevel.value = Math.max(minZoom, zoomLevel.value - 0.1)
  }
}
```

每次增减 0.1，用 `Math.min` 和 `Math.max` 限制范围。

#### 重置视图

```typescript
const resetView = () => {
  zoomLevel.value = 1
  position.value = { x: 0, y: 0 }
}

// 切换页面时重置位置
watch(page, () => {
  position.value = { x: 0, y: 0 }
})
```

切换页面时也会自动重置位置，避免上一页的拖拽状态影响新页面。

#### 判断是否需要重置

```typescript
const needsReset = computed(() => {
  return zoomLevel.value !== 1 || position.value.x !== 0 || position.value.y !== 0
})
```

只有当缩放不是 1 或位置不是原点时，才显示重置按钮。

## 一些细节

### 页码控制

#### 上一页/下一页

```typescript
const prevPage = () => {
  if (page.value > 1) {
    page.value--
  }
}

const nextPage = () => {
  if (page.value < totalPages.value) {
    page.value++
  }
}
```

#### 页码输入处理

```typescript
const handlePageChange = (value) => {
  if (typeof value === 'string') {
    value = parseInt(value)
  }
  const newPage = parseInt(value)
  if (isNaN(newPage) || newPage < 1) {
    page.value = 1
  } else if (newPage > totalPages.value) {
    page.value = totalPages.value
  } else {
    page.value = newPage
  }
}
```

处理边界情况：非数字、小于 1、大于总页数。

### 全屏模式

#### 切换全屏

```typescript
const toggleFullscreen = async () => {
  try {
    if (!document.fullscreenElement) {
      await contentRef.value.requestFullscreen()
    } else {
      await document.exitFullscreen()
    }
  } catch (err) {
    ElMessage.error('Fullscreen mode is not supported or disabled')
  }
}
```

#### 监听全屏状态

```typescript
const handleFullscreenChange = () => {
  isFullscreen.value = !!document.fullscreenElement
}

// 在 onMounted 中监听
onMounted(() => {
  document.addEventListener('fullscreenchange', handleFullscreenChange)
})

// 在 onUnmounted 中移除监听
onUnmounted(() => {
  document.removeEventListener('fullscreenchange', handleFullscreenChange)
  // 确保退出组件时退出全屏
  if (document.fullscreenElement) {
    document.exitFullscreen()
  }
})
```

### 容器尺寸监听

```typescript
const updateContainerSize = () => {
  if (pdfContainerRef.value) {
    const container = pdfContainerRef.value
    containerSize.value = {
      width: container.offsetWidth,
      height: container.offsetHeight,
    }
  }
}

// 监听窗口大小变化
onMounted(() => {
  window.addEventListener('resize', updateContainerSize)
})

onUnmounted(() => {
  window.removeEventListener('resize', updateContainerSize)
})
```

### 性能优化

- 标记层用 `pointer-events: none`，只有标记本身响应鼠标事件
- transform 用 CSS 实现，利用 GPU 加速
- 切换页面时重置位置，避免不必要的计算
- 用 computed 缓存计算结果，减少重复计算

## 总结

整体实现不算复杂，主要是几个点：
1. PDF 渲染用现成的库，省事
2. 标记用 overlay 层叠加，不污染 PDF 本身
3. 坐标用相对值，适配各种缩放比例
4. 缩放和拖拽配合，提升用户体验

实际用起来效果还行，标记位置准确，缩放拖拽也流畅。就是标记多的时候可能会有性能问题，不过目前还没遇到。

## CSS 样式详解

样式这块也挺重要，直接影响交互体验。下面详细说说关键样式的作用。

### 整体布局

```scss
.content {
  display: grid;
  grid-template-columns: 300px 1fr;  // 左侧文件列表 300px，右侧 PDF 区域自适应
  margin-bottom: 8px;
  height: 100%;
}
```

用 Grid 布局，左侧固定宽度，右侧自适应。

### PDF 容器样式

```scss
.content-body {
  flex: 1;
  overflow: hidden;              // 隐藏溢出内容
  display: flex;
  border-radius: 10px;
  border: 2px solid #bdbdbd;
  position: relative;
  margin: 10px;

  // 全屏模式
  &.fullscreen {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 9999;               // 确保在最上层
    margin: 0;
    border-radius: 0;
    background-color: white;
  }
}
```

全屏时用 `position: fixed` 铺满整个屏幕，`z-index: 9999` 确保在最上层。

### PDF 视图区域

```scss
.pdf-view-area {
  flex: 1;
  position: relative;
  border-right: 1px solid #dcdfe6;
  display: flex;
  justify-content: center;        // 水平居中
  align-items: center;            // 垂直居中
  overflow: hidden;               // 隐藏溢出，配合拖拽使用
}
```

`overflow: hidden` 很重要，这样放大后拖拽时，超出容器的部分会被隐藏。

### PDF 内容区域

```scss
.pdf-content {
  max-width: 100%;
  max-height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  position: relative;
  transition: transform 0.1s ease;  // transform 动画，让缩放更平滑
  user-select: none;                // 防止拖动时选中文本

  &:active {
    cursor: grabbing;               // 拖拽时显示抓取光标
  }

  // PDF canvas 样式
  :deep(canvas) {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;           // 保持比例，完整显示
  }
}
```

`user-select: none` 防止拖拽时选中文本，`transition` 让缩放动画更平滑。

### 标注叠加层

```scss
.annotations-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: none;            // 不拦截鼠标事件

  .annotation-marker {
    position: absolute;
    border: 2px solid;
    border-radius: 4px;
    pointer-events: auto;          // 标记本身可以响应鼠标事件
    cursor: pointer;
    opacity: 0.6;                  // 默认半透明
    transition: opacity 0.3s;     // 透明度过渡

    &:hover {
      opacity: 1;                  // 悬停时完全不透明
    }

    // 不同状态的样式
    &.error {
      border-color: #f56c6c;
      background-color: rgba(245, 108, 108, 0.3);
    }

    &.warning {
      border-color: #e6a23c;
      background-color: rgba(230, 162, 60, 0.3);
    }

    &.success {
      border-color: #67c23a;
      background-color: rgba(103, 194, 58, 0.3);
    }
  }
}
```

`pointer-events: none` 让 overlay 不拦截事件，但标记本身设置 `pointer-events: auto`，这样只有标记能响应鼠标。用 `rgba` 设置半透明背景，不会完全遮挡 PDF 内容。

### 标注列表样式

```scss
.file-annotation {
  width: 300px;
  padding: 16px;
  background-color: #f5f7fa;
  display: flex;
  flex-direction: column;
  justify-content: space-between;

  .annotation-list {
    flex: 1;
    overflow-y: auto;              // 列表可滚动
    margin-bottom: 16px;

    .annotation-item {
      background-color: #fff;
      padding: 12px;
      border-radius: 4px;
      margin-bottom: 12px;
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
      display: flex;
      align-items: center;
      gap: 12px;
      cursor: pointer;
      transition: all 0.3s;        // 所有属性过渡

      &:hover {
        transform: translateX(-4px);  // 悬停时向左移动
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
      }

      &.highlighted {
        background-color: #ecf5ff;
        border-left: 4px solid #409eff;  // 左侧蓝色边框
      }

      .annotation-status {
        width: 12px;
        height: 12px;
        border-radius: 50%;         // 圆形状态点

        &.error {
          background-color: #f56c6c;
        }

        &.warning {
          background-color: #e6a23c;
        }

        &.success {
          background-color: #67c23a;
        }
      }
    }
  }
}
```

标注项悬停时用 `transform: translateX(-4px)` 向左移动，配合阴影变化，有轻微的"浮起"效果。高亮时左侧加蓝色边框，视觉上更明显。

### 控制栏样式

```scss
.pdf-controls {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);    // 水平居中
  background-color: rgba(255, 255, 255, 0.9);  // 半透明白色背景
  padding: 10px;
  border-radius: 8px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
  gap: 20px;
  z-index: 100;                    // 确保在 PDF 上方

  .controls-group {
    display: flex;
    align-items: center;
    gap: 10px;

    &.zoom-controls {
      padding-left: 10px;
      border-left: 1px solid #dcdfe6;  // 左侧分隔线
    }

    .control-icon {
      font-size: 20px;
      padding: 8px;
      border-radius: 4px;
      cursor: pointer;
      transition: all 0.3s;
      color: #409eff;

      &:hover {
        background-color: #ecf5ff;
      }

      &.disabled {
        color: #c0c4cc;
        cursor: not-allowed;

        &:hover {
          background-color: transparent;  // 禁用时不响应悬停
        }
      }
    }

    .zoom-level {
      min-width: 60px;
      text-align: center;
      font-size: 14px;
      color: #606266;
    }
  }
}
```

控制栏用 `position: absolute` 固定在底部，`transform: translateX(-50%)` 实现水平居中。半透明背景不会完全遮挡 PDF，但又能看清控制按钮。

### 页码输入框样式

```scss
.page-input-group {
  display: flex;
  align-items: center;
  margin: 0 10px;

  .el-input {
    width: 55px;
    margin-right: 5px;

    :deep(.el-input__inner) {
      padding: 0 8px;
      text-align: center;          // 文字居中
    }

    // 隐藏数字输入框的上下箭头
    :deep(.el-input__inner::-webkit-outer-spin-button),
    :deep(.el-input__inner::-webkit-inner-spin-button) {
      -webkit-appearance: none;
      margin: 0;
    }

    :deep(.el-input__inner[type='number']) {
      -moz-appearance: textfield;  // Firefox 隐藏箭头
    }
  }
}
```

隐藏数字输入框的上下箭头，让界面更简洁。用 `:deep()` 穿透 Element Plus 的样式作用域。

### 文件列表样式

```scss
.files-section {
  padding: 10px;
  flex: 1;
  display: flex;
  flex-direction: column;

  &.hidden-in-fullscreen {
    display: none;                 // 全屏时隐藏
  }

  .file-list {
    flex: 1;
    overflow-y: auto;              // 可滚动
    border-radius: 10px;
    border: 2px solid #e7e7e7;

    .file-item {
      padding: 12px 16px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      transition: background-color 0.2s;
      border-bottom: 2px solid #e7e7e7;

      &:hover {
        background: #f5f7fa;
      }

      &.active {
        background: #0085fc;      // 选中时蓝色背景
        color: #fff;

        .file-icon {
          color: #fff;
        }
      }
    }
  }
}
```

文件项选中时用蓝色背景，视觉上更明显。悬停时背景色变化，提供交互反馈。

### 样式技巧总结

1. **定位技巧**：PDF 容器用 `position: relative`，标注 overlay 用 `position: absolute` 叠加
2. **事件穿透**：overlay 用 `pointer-events: none`，标记用 `pointer-events: auto`
3. **居中技巧**：用 `transform: translateX(-50%)` 实现绝对定位元素的水平居中
4. **过渡动画**：用 `transition` 让交互更平滑，特别是 transform 和 opacity
5. **层级管理**：用 `z-index` 控制元素层级，控制栏和全屏模式需要较高层级
6. **响应式**：用 `max-width` 和 `max-height` 配合 `object-fit: contain` 保持比例


## 一些其他的预览 PDF 方法

1. 使用 @vue-office/pdf 库
2. 微软官方的 Office Online 预览 （https://view.officeapps.live.com/op/view.aspx?src=https://501351981.github.io/vue-office/examples/dist/static/test-files/test.docx） 需要第三方域名支持
