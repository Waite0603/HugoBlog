---
title: "Python 实践网页报告生成与邮件分发"
date: 2025-05-07T16:04:23+08:00
categories: ["Python"]
tags: ["Python"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---
## 项目背景

在现代化应用中，自动化报告生成和邮件分发是常见的需求。本文将介绍两个 Python 工具模块：`long_screenshot.py` 和 `send_email.py`，它们分别负责网页长截图和邮件发送功能，可以协同工作，实现从网页截图到邮件分发的完整流程。

## 工具模块介绍

### 1. 网页长截图模块 (`long_screenshot.py`)

该模块使用 `playwright` 和 `img2pdf` 实现网页长截图并生成 PDF 文件。

#### 核心功能：
- 支持自定义设备模拟（如 iPhone 15）
- 异步实现，适合与异步框架集成
- 自动计算页面实际高度
- 生成高质量 PDF 文件

#### 关键代码分析：
```python
async def take_long_screenshot(url: str, output_path: str = "output.pdf", device_name: str = "iPhone 15") -> None:
    # 使用异步上下文管理器启动 playwright
    async with async_playwright() as p:
        # 启动无头浏览器（headless=True 表示不显示浏览器界面）
        browser = await p.chromium.launch(headless=True)
        # 获取设备配置（如 iPhone 15 的屏幕尺寸、UA 等）
        device = p.devices[device_name]
        # 创建新的浏览器上下文，应用设备配置
        context = await browser.new_context(**device)
        # 创建新页面
        page = await context.new_page()

        try:
            # 访问目标 URL，等待网络请求完成
            await page.goto(url, wait_until="networkidle")
            # 等待 2 秒，确保页面完全加载
            await page.wait_for_timeout(2000)

            # 计算页面实际高度
            # 1. 设置页面溢出为可见
            # 2. 遍历所有元素，找出最底部的位置
            # 3. 返回实际高度
            real_height = await page.evaluate("""() => {
                document.body.style.overflow = 'visible';
                document.documentElement.style.overflow = 'visible';
                const elements = document.getElementsByTagName('*');
                let maxHeight = 0;
                for (let element of elements) {
                    const rect = element.getBoundingClientRect();
                    maxHeight = Math.max(maxHeight, rect.bottom);
                }
                return Math.ceil(maxHeight);
            }""")

            # 设置视口大小，确保能完整显示页面
            await page.set_viewport_size({
                "width": device["viewport"]["width"],
                "height": real_height
            })

            # 等待 10 秒，确保动态内容加载完成
            await page.wait_for_timeout(10000)

            # 截取整个页面
            screenshot = await page.screenshot(
                full_page=True,
                type="png",
                clip={
                    "x": 0,
                    "y": 0,
                    "width": device["viewport"]["width"],
                    "height": real_height
                }
            )

            # 将截图转换为 PDF
            with open(output_path, "wb") as pdf_file:
                pdf_file.write(img2pdf.convert(screenshot))

        except Exception as e:
            print(f"截图失败: {str(e)}")
            raise
        finally:
            # 确保浏览器关闭，释放资源
            await browser.close()
```

#### 性能优化建议：
1. 调整等待时间：
   ```python
   # 根据页面复杂度调整等待时间
   await page.wait_for_timeout(2000)  # 基础等待
   await page.wait_for_timeout(10000)  # 动态内容等待
   ```

2. 使用选择器等待：
   ```python
   # 等待特定元素加载完成
   await page.wait_for_selector('.report-content', timeout=30000)
   ```

3. 优化内存使用：
   ```python
   # 分块处理大页面
   chunk_height = 1000
   for y in range(0, real_height, chunk_height):
       screenshot = await page.screenshot(
           clip={
               "x": 0,
               "y": y,
               "width": device["viewport"]["width"],
               "height": min(chunk_height, real_height - y)
           }
       )
   ```

### 2. 邮件发送模块 (`send_email.py`)

该模块实现了邮件发送功能，支持同步和异步两种方式，并支持带 PDF 附件的邮件发送。

#### 核心功能：
- 支持 HTML 格式邮件内容
- 自动添加签名和退订信息
- 支持 PDF 附件
- 异步发送支持回调通知

#### 关键代码分析：
```python
def _create_email_message(
        message: str,
        Subject: str,
        sender_show: str,
        recipient_show: str,
        to_addrs: str,
        cc_show: str = '',
        pdf_path: str = 'output.pdf'
) -> MIMEMultipart:
    # 创建邮件容器
    msg = MIMEMultipart('alternative')
    
    # 设置邮件头
    msg['Message-ID'] = email.utils.make_msgid()
    msg['Date'] = email.utils.formatdate(time.time(), True)
    msg['From'] = email.utils.formataddr((str(Header(sender_show, 'utf-8')), user))
    msg['To'] = email.utils.formataddr((str(Header(recipient_show, 'utf-8')), to_addrs))
    msg['Subject'] = Header(Subject, 'utf-8')
    
    # 添加邮件内容
    html_content = f"""
    <html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    </head>
    <body>
        {message}
        {footer}
    </body>
    </html>
    """
    msg.attach(MIMEText(html_content, 'html', 'utf-8'))
    
    # 添加 PDF 附件
    try:
        with open(pdf_path, 'rb') as pdf_file:
            pdf_attachment = MIMEApplication(pdf_file.read(), _subtype='pdf')
            pdf_attachment.add_header('Content-Disposition', 'attachment',
                                      filename=('utf-8', '', '名书报告.pdf'))
            msg.attach(pdf_attachment)
    except FileNotFoundError:
        print(f"Warning: {pdf_path} not found, sending email without attachment")
    
    return msg

def _send_email_worker(
        msg: MIMEMultipart,
        to_addrs: str,
        callback: Optional[Callable[[bool, Optional[Exception]], None]] = None
) -> None:
    try:
        # 使用 SSL 连接 SMTP 服务器
        with SMTP_SSL(host=config.EMAIL_SMTP_HOST, port=config.EMAIL_SMTP_PORT) as smtp:
            # 登录
            smtp.login(user=config.EMAIL_USER, password=config.EMAIL_PASSWORD)
            # 发送邮件
            smtp.sendmail(
                from_addr=config.EMAIL_USER,
                to_addrs=[addr.strip() for addr in to_addrs.split(',')],
                msg=msg.as_string()
            )
            print("Email sent successfully")
            if callback:
                callback(True, None)
    except Exception as e:
        print(f"Failed to send email: {e}")
        if callback:
            callback(False, e)
        raise
```

#### 实际使用示例：
```python
# 同步发送邮件
def send_report_sync(report_url: str, recipient_email: str):
    try:
        # 生成 PDF
        pdf_path = "report.pdf"
        asyncio.run(take_long_screenshot(report_url, pdf_path))
        
        # 发送邮件
        message = """
        <h3>您的报告已生成</h3>
        <p>请查看附件获取详细报告。</p>
        """
        sendMail(
            message=message,
            Subject="您的报告",
            sender_show="系统",
            recipient_show="用户",
            to_addrs=recipient_email,
            pdf_path=pdf_path
        )
    except Exception as e:
        print(f"发送失败: {e}")
        # 处理错误...

# 异步发送邮件
def send_report_async(report_url: str, recipient_email: str):
    async def generate_report():
        pdf_path = "report.pdf"
        await take_long_screenshot(report_url, pdf_path)
        return pdf_path
    
    def on_email_sent(success: bool, error: Optional[Exception]):
        if success:
            print("邮件发送成功")
        else:
            print(f"邮件发送失败: {error}")
    
    # 使用 asyncio 运行异步任务
    pdf_path = asyncio.run(generate_report())
    
    # 异步发送邮件
    sendMailAsync(
        message="<h3>您的报告已生成</h3>",
        Subject="您的报告",
        sender_show="系统",
        recipient_show="用户",
        to_addrs=recipient_email,
        pdf_path=pdf_path,
        callback=on_email_sent
    )
```

## 配置说明

### 邮件配置 (`config.py`)
```python
# 邮件服务器配置
EMAIL_USER = "your_email@example.com"
EMAIL_PASSWORD = "your_password"
EMAIL_SMTP_HOST = "smtp.example.com"
EMAIL_SMTP_PORT = 465

# 可选：配置重试机制
EMAIL_MAX_RETRIES = 3
EMAIL_RETRY_DELAY = 5  # 秒
```

### 依赖安装
```bash
# 安装必要的包
pip install playwright img2pdf

# 安装浏览器驱动
playwright install chromium
```

## 总结与最佳实践

1. 技术要点：
   - 使用 `playwright` 实现高质量网页截图
   - 异步处理提高系统性能
   - 模块化设计便于维护和扩展

2. 最佳实践：
   - 合理设置超时时间
   - 添加错误处理和日志记录
   - 使用配置文件管理敏感信息
   - 考虑邮件发送频率限制

3. 注意事项：
   - 确保 SMTP 服务器配置正确
   - 注意网页加载时间
   - 处理大文件时的内存占用

## 常见问题解答

1. Q: 如何处理网页加载超时？
   A: 调整 `wait_for_timeout` 参数，或使用 `wait_for_selector` 等待特定元素。

2. Q: 邮件发送失败如何处理？
   A: 使用回调函数捕获异常，实现重试机制。

3. Q: 如何优化大文件处理？
   A: 考虑使用流式处理或分块上传。


## 参考资料

- [Playwright 官方文档](https://playwright.dev/python/)
- [img2pdf 文档](https://pypi.org/project/img2pdf/)
- [Python SMTP 文档](https://docs.python.org/3/library/smtplib.html) 