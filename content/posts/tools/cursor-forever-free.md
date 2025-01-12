---
title: "Cursor 无限续杯方案"
date: 2025-01-12T20:55:23+08:00
lastmod: 2025-01-12T20:55:23+08:00
categories: ["Blog"]
tags: ["Blog"]
author: "Waite Wang"
showToc: true
TocOpen: true
draft: false
hidemeta: false
description: "Cursor 无限续杯."
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

> 参考地址：<https://github.com/chengazhen/cursor-auto-free>

在本篇文章开始之前，确保你已经安装好了 Cursor[https://www.cursor.com/]

本篇文章主要介绍[cursor-auto-free](https://github.com/chengazhen/cursor-auto-free)这个项目的使用。

## 源代码构建或者下载执行文件

我们可以通过 `git clone` 命令来下载源代码，或者直接在 Release 页面下载已经构建好的执行文件。

然后我们根据项目配置要求，配置变量

```bash
需要使用 cloudflare 域名邮箱，请自行搜索如何使用 cloudflare 域名邮箱，请自行搜索如何使用。
（非常重要） 需要使用 temp-mail.plus 邮箱，请自行搜索如何使用 temp-mail.plus 邮箱。
将 cloudflare 的域名邮箱转发到 temp-mail.plus 邮箱。
下载 .env.example 文件到程序所在根目录，并重命名为 .env 文件。
修改 .env 文件中的配置。
```

下文将告诉你如何配置这些变量。

## 配置变量

使用Cloudflare的前提是要有一个域名。

有了域名以后，我们就把它托管到Cloudflare上。我们先登录一下Cloudflare，没有账号就注册一个。右上角点击添加站点，输入你的域名，然后点击继续。这里有一些付费的，我们都不要，直接找下面这个免费的，点击继续。

![](https://qiniu.waite.wang/202501122103640.png)

一路下一步。这步是重点，更改名称服务器。找到这两个名称服务器。

![](https://qiniu.waite.wang/202501122104630.png)

我们需要去刚才买域名的网站更改一下DNS地址r。就把这里改一下，把原来的全部删掉，然后填上刚才网站给我分配的这两个DNS，点击保存。

![](https://qiniu.waite.wang/202501122105730.png)

我们再回到Cloudflare，点击立即检查，等待一段时间后，显示成功或者当cloudflare 发送成功邮件到你的邮箱，就表示成功了。

在验证成功后，我们就可以使用 Cloudflare 的域名邮箱了。

在任务栏，点击电子邮件，启用电子邮件，根据提示下一步即可。

成功开通后，点击路由规则，这里有一个catch-all地址，我们点击右侧的编辑，然后点击添加规则，选择转发到，然后填上我们的 [temp-mail.plus](https://tempmail.plus/zh/) 邮箱地址，点击保存。

这时我们在 temp-mail.plus 邮箱中就可以收到 Cloudflare 的验证邮件，点击验证即可。

当显示以下时表示验证成功，且可以正常使用。

![](https://qiniu.waite.wang/202501122114363.png)

这时我们把我们域名邮箱和 temp-mail.plus 邮箱前缀填入到 `.env` 文件中，运行 `cursor-pro-keep-alive.py`/ 可执行文件 即可。

![](https://qiniu.waite.wang/202501122116314.png)

## 运行中可能遇到的问题

![](https://qiniu.waite.wang/202501122118375.png)

这是因为 Google 浏览器不正常安装导致的，如果确定已经安装了 Google 浏览器，更改如下代码即可。

```python
# browser_utils.py
from DrissionPage import ChromiumOptions, Chromium
import sys
import os
import logging


class BrowserManager:
  def __init__(self):
    self.browser = None
    self.browser_executable_path = None

  def set_browser_path(self, path):
    """Set the path to the browser executable."""
    self.browser_executable_path = path

  def init_browser(self):
    """Initialize the browser."""
    co = self._get_browser_options()
    self.browser = Chromium(co)
    return self.browser

  def _get_browser_options(self):
    """获取浏览器配置"""
    co = ChromiumOptions()
    try:
      extension_path = self._get_extension_path()
      co.add_extension(extension_path)
    except FileNotFoundError as e:
      logging.warning(f"Warning: {e}")

    co.set_user_agent(
      "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.92 "
      "Safari/537.36"
    )
    co.set_pref("credentials_enable_service", False)
    co.set_argument("--hide-crash-restore-bubble")
    co.auto_port()
    co.headless(True)  # Use headless mode in production

    # Special handling for Mac systems
    if sys.platform == "darwin":
      co.set_argument("--no-sandbox")
      co.set_argument("--disable-gpu")

    # Set the browser executable path if provided
    if self.browser_executable_path:
      co.set_browser_path(self.browser_executable_path)

    return co

  def _get_extension_path(self):
    """Get the extension path."""
    root_dir = os.getcwd()
    extension_path = os.path.join(root_dir, "turnstilePatch")

    if hasattr(sys, "_MEIPASS"):
      extension_path = os.path.join(sys._MEIPASS, "turnstilePatch")

    if not os.path.exists(extension_path):
      raise FileNotFoundError(f"Extension not found: {extension_path}")

    return extension_path

  def quit(self):
    """Close the browser."""
    if self.browser:
      try:
        self.browser.quit()
      except:
        pass
```

```python
# cursor_pro_keep_alive.py
if __name__ == "__main__":
    print_logo()
    browser_manager = None
    try:
        logging.info("\n=== 初始化程序 ===")
        ExitCursor()
        logging.info("正在初始化浏览器...")
        browser_manager = BrowserManager()
        # 添加以下这行，手动设置浏览器路径
        browser_manager.set_browser_path("C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe")
        browser = browser_manager.init_browser()
```
