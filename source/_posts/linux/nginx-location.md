---
title: nginx location块配置中斜杠"/"的路径匹配规则
date: 2023-11-06 14:35:16
sticky: true
categories: # 分类
- linux
tags: # 标签
- nginx
---

# 前言

# nginx规则分类
```bash nginx规则分类
＝   精确匹配                   （优先级最高）# 只匹配根目录结尾的请求，后面不能带任何字符串
^~   精确前缀匹配               （优先级仅次于=）
~    区分大小写的正则匹配       （优先级次于^~）
~*   不区分大小写的正则匹配     （优先级次于^~）
/uri 普通前缀匹配               （优先级次于正则）# 最常用
/    通用匹配                   （优先级最低）# 默认匹配路径
```

# 下面是测试location块中"`/`"的匹配案例
:::primary
前置测试访问域名：http://127.0.0.1/api/test
:::
---
## location和proxy_pass`都不带/`，则真实地址会带location匹配目录/api/
   ```bash location配置
    location /api {
        proxy_pass http://127.0.0.1:8080;
    }
   ```
   :::info
   最终访问地址： http://127.0.0.1[/api/test]{.pink} --> http://127.0.0.1:8080[/api/test]{.pink}
   :::
---
## location`带/`，proxy_pass`不带/`，则真实地址会带location匹配目录/api/
   ```bash location配置
    location /api/ {
        proxy_pass http://127.0.0.1:8080;
    }
   ```
   :::info
   最终访问地址： http://127.0.0.1[/api/test]{.pink} --> http://127.0.0.1:8080[/api/test]{.pink}
   :::
---
## location`不带/`，proxy_pass`带/`，则真实地址会`带/`
```bash location配置
location /api {
    proxy_pass http://127.0.0.1:8080/;
}
```
:::info
最终访问地址：http://127.0.0.1[/api/test]{.pink} --> http://127.0.0.1:8080[//test]{.pink}
:::
---
## location和proxy_pass`都带/`，则真实地址不带location匹配目录
```bash location配置
location /api/ {
    proxy_pass http://127.0.0.1:8080/;
}
```
:::info
最终访问地址：http://127.0.0.1[/api/test]{.pink} --> http://127.0.0.1:8080[/test]{.pink}
:::
---
## 同1，location和proxy_pass`都不带/`，但 proxy_pass`带地址`，则真实地址匹配地址会替换location匹配目录
   ```bash location配置
    location /api {
        proxy_pass http://127.0.0.1:8080/server;
    }
   ```
   :::info
   最终访问地址： http://127.0.0.1[/api/test]{.pink} --> http://127.0.0.1:8080[/server/test]{.pink}
   :::
---
## 同2，location`带/`，proxy_pass`不带/`，但 proxy_pass`带地址`，则真实地址会直接连起来
   ```bash location配置
    location /api/ {
        proxy_pass http://127.0.0.1:8080/server;
    }
   ```
   :::info
   最终访问地址： http://127.0.0.1[/api/test]{.pink} --> http://127.0.0.1:8080[/servertest]{.pink}
   :::
---
## 同2，location`不带/`，proxy_pass`带/`，但 proxy_pass`带地址`，则真实地址会多个`/`
   ```bash location配置
    location /api {
        proxy_pass http://127.0.0.1:8080/server/;
    }
   ```
   :::info
   最终访问地址： http://127.0.0.1[/api/test]{.pink} --> http://127.0.0.1:8080[/server//test]{.pink}
   :::
---
## 同4，location和proxy_pass`都带/`，但 proxy_pass`带地址`
   ```bash location配置
    location /api/ {
        proxy_pass http://127.0.0.1:8080/server/;
    }
   ```
   :::info
   最终访问地址： http://127.0.0.1[/api/test]{.pink} --> http://127.0.0.1:8080[/server/test]{.pink}
   :::
---
#  总结
1. proxy_pass代理地址端口后++有目录++{.wavy}(包括`/`)，则`去除location`匹配目录
2. proxy_pass代理地址端口后无任何，转发后地址：代理地址+访问URL目录部