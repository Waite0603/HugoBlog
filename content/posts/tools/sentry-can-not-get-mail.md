---
title: "Sentry 的安装部署以及邮件无法发送问题解决"
date: 2025-12-17T10:05:54+08:00
lastmod: 2025-12-17T10:05:54+08:00
categories: ["Tools"]
tags: ["Tools", "Sentry"]
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

## 安装

> 注意 这里安装的是 Sentry 9.1.2 版本，该版本是 Docker 仓库支持的最后一个版本，对服务器要求较低。如果服务器支持，可以安装最新版本，安装方法会有所不同，具体可以参考官方文档 https://develop.sentry.dev/self-hosted/

```bash
docker pull sentry  
docker pull redis 6.2-alpine 
docker pull postgres 13-alpine

# 打包进入服务器， 启动 redis 和 postgres
docker run -d --name sentry-redis --restart=always redis:6.2-alpine
docker run -d --name sentry-postgres \
  -e POSTGRES_PASSWORD=secret \
  -e POSTGRES_USER=sentry \
  --restart=always \
  postgres:13-alpine

# 生成sentry秘钥，生成的秘钥后面步骤中都需要填写，很重要
docker run --rm sentry config generate-secret-key

# 数据库及账户初始化,过程中需要创建用户和密码，用于Sentry系统的登录
docker run -it --rm -e SENTRY_SECRET_KEY='生成的密码' --link sentry-postgres:postgres --link sentry-redis:redis sentry upgrade

# 启动 Sentry 服务
 docker run -d --name my-sentry \
-p 9000:9000 \
-e SENTRY_SECRET_KEY='生成的密码' \
-e SENTRY_EMAIL_HOST='smtp.exmail.qq.com' \
-e SENTRY_EMAIL_PORT='465' \
-e SENTRY_EMAIL_USER='你的邮箱' \
-e SENTRY_EMAIL_PASSWORD='你的密码' \
-e SENTRY_EMAIL_USE_TLS=True \
-e SENTRY_SERVER_EMAIL='你的邮箱' \
--link sentry-postgres:postgres \
--link sentry-redis:redis sentry

docker run -d --name sentry-worker \
-e SENTRY_SECRET_KEY='生成的密码' \
-e SENTRY_EMAIL_HOST='smtp.exmail.qq.com' \
-e SENTRY_EMAIL_PORT='465' \
-e SENTRY_EMAIL_USER='你的邮箱' \
-e SENTRY_EMAIL_PASSWORD='你的密码' \
-e SENTRY_EMAIL_USE_TLS=True \
-e SENTRY_SERVER_EMAIL='你的邮箱' \
--link sentry-postgres:postgres \
--link sentry-redis:redis sentry run  worker

docker run -d --name sentry-cron  \
-e SENTRY_SECRET_KEY='生成的密码' \
-e SENTRY_EMAIL_HOST='smtp.exmail.qq.com' \
-e SENTRY_EMAIL_PORT='465' \
-e SENTRY_EMAIL_USER='你的邮箱' \
-e SENTRY_EMAIL_PASSWORD='你的密码' \
-e SENTRY_EMAIL_USE_TLS=True \
-e SENTRY_SERVER_EMAIL='你的邮箱' \
--link sentry-postgres:postgres \
--link sentry-redis:redis sentry run cron
```

> 至此 Sentry 服务已经启动，可以通过 http://服务器IP:9000 访问，使用上面初始化时创建的用户登录

## 邮件无法发送问题解决

> sentry环境用的是python2.7，发邮件使用的是django的email，使用的版本是django 1.6，而django1.7才支持使用ssl。
也就是说，sentry配置默认不支持ssl邮件发送。 而由于25端口（非ssl)默认被封禁，所以服务器也不能使用25端口发送。具体可以查看 [https://github.com/getsentry/sentry/issues/4252](https://github.com/getsentry/sentry/issues/4252)

1. 使用 `docker ps` 查看正在运行的sentry容器ID

```bash
docker ps

a219e29e75b5   sentry                                  "/entrypoint.sh run …"   16 hours ago   Up 16 hours             9000/tcp                                      sentry-cron
22bb5e936bc9   sentry                                  "/entrypoint.sh run …"   16 hours ago   Up 3 seconds            9000/tcp                                      sentry-worker
1db8e3a941c8   sentry                                  "/entrypoint.sh run …"   16 hours ago   Up 6 minutes            0.0.0.0:9000->9000/tcp, [::]:9000->9000/tcp   my-sentry
```

> 我们需要修改 `my-sentry` 以及 `sentry-worker` 两个容器

2. 先把其中一个的配置文件拷贝到宿主机

```bash
docker cp <container_name>:/etc/sentry/config.yml ./config.yml
```

3. 修改 `config.yml` 文件，添加如下配置

```yaml
mail.backend: 'django_smtp_ssl.SSLEmailBackend'
mail.host: 'smtp.exmail.qq.com'
mail.port: 465
mail.username: '你的邮箱地址'
mail.password: '你的邮箱密码'
mail.use-tls: true
# The email address to send on behalf of
mail.from: '你的邮箱地址'
```

4. 把修改后的配置文件拷贝回容器, 注意两个容器都要拷贝

```bash
docker cp ./config.yml <container_name>:/etc/sentry/config.yml
```

5. 分别进入两个容器, 安装 `django-smtp-ssl` 包

```bash
docker exec -it <container_name> /bin/bash
pip install django-smtp-ssl
exit
```

6. 重启容器

```bash
docker restart <container_name>
```

7. 使用 `docker ps` 查看容器是否重启成功, 如果成功, 可以尝试发送测试邮件以及邀请邮件~