# 关于本页

Shodan 索引了它在互联网上能连到的每一台主机，并能就它们回答问题。它本来就有 **39 个查询 facet** —— 也就是能把一次搜索拆开的那些维度：

```
asn  city  country  cpe  domain  has_screenshot  hash
http.component  http.component_category  http.favicon.hash  http.html_hash
http.robots_hash  http.status  http.title  http.waf
ip  isp  mongodb.database.name  org  os  port  product
redis.key  region  rsync.module  screenshot.label
ssh.fingerprint  ssl.alpn  ssl.cert.fingerprint  ssl.cert.issuer.cn
ssl.cert.serial  ssl.cert.subject.cn  ssl.ja3s  ssl.jarm  ssl.version
state  tag  version  vuln
```

Shodan 缺的不是数据，而是**把答案交到别人手上时变成一张图的办法**。facet 分解出来是 JSON；而一坨 JSON 扔进 GitHub issue、博客或者聊天窗口，别人只会划过去。

这个页面做的就是这件事：**把 facet 分解渲染出来**。又因为图的 URL 就是参数，查询是可以替换的 —— 把 `product:nginx` 换成 `org:你的公司` 或 `net:10.0.0.0/8`，同一个模板立刻变成*你自己*攻击面的图。

> CLI：`x shodan` —— 见 [x-cmd.com/mod/shodan](https://x-cmd.com/mod/shodan)
> 数据：[api.shodan.io](https://api.shodan.io) · [internetdb.shodan.io](https://internetdb.shodan.io)（免 key）
> 下面所有数字都是 **2026-10-29** 查询的，会随时间漂移。
