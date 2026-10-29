# Shodan 度量的是什么，这个页面拿它做什么

## Shodan 和搜索引擎的区别

网页搜索引擎问的是：*哪些页面在说什么？* Shodan 问的是另一个问题：*有什么在跑？* 它去连主机，看它们暴露了什么 —— 开放的端口、每个服务发出的 banner、TLS 证书、HTTP 头、操作系统指纹。它没有页面索引，它有的是一份**主机及其所运行服务的索引**。

所以 Shodan 的结果围绕 **facet** 而不是关键词组织。facet 就是一个可以把结果集拆开的维度。Shodan 公开了 39 个：

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

把这份清单当作一份"互联网的 schema"来读：东西**在哪里**（`country`、`city`、`asn`、`org`、`isp`），**跑着什么**（`port`、`product`、`version`、`cpe`），**底座是什么**（`os`），**对外提供什么**（`http.title`、`http.status`、`http.waf`），**用什么加密**（`ssl.jarm`、`ssl.ja3s`、`ssl.cert.*`、`ssl.version`），**复用了什么密钥**（`ssh.fingerprint`），以及**哪里有毛病**（`vuln`）。

## 这个页面补的是哪个缺口

Shodan 的输出有一点很容易被忽略：**facet 分解是以 JSON 形式回来的。** 而一坨 JSON 扔进 GitHub issue、聊天窗口或博客，别人只会划过去。表格里的数字好一点；图更好 —— 因为**分布的形状往往才是发现本身，而 JSON 把形状藏起来了**。

所以这个页面只做一件事：**把 facet 分解渲染出来**。而且不是截图 —— 是一张数据仍然可查询的图，因为这里的"图"就是一个把参数写出来的 URL。

而最后这一条才是重点。因为 URL 是可编辑的，**查询就是可编辑的**。把 `product:nginx` 换成 `org:你的公司` 或 `net:10.0.0.0/8`，同一个模板就成了*你自己*攻击面的图，而不是 nginx 的图。Shodan 自己的用户可以照着本页底部的 facet 表自己动手。

## 这个站其余部分怎么读

- **暴露面（exposure）** —— 互联网上有多少在跑某个服务、这个数字怎么变、集中在哪里。
- **漏洞（vulnerabilities）** —— 哪些已知漏洞仍然可达，其中哪些是被人**故意**在利用的。
- **指纹（fingerprints）** —— TLS 与 SSH 层能透露什么、而证书不能透露什么。

## 范围与诚实

这里每一个数字都是 **2026-10-29** 向 Shodan 的 API 查出来的，会随时间漂移。Shodan 数的是*它自己扫到过*的东西，所以它的覆盖度是 Shodan 扫描机队和观测点的一个属性 —— 不是对互联网的普查。这些计数最好当作**数量级和比值**来读，而不是精确总数。

凡是某个数字带有特定偏差的（IPv4 分配数是*分配条目*而不是地址数；国别分布反映的是托管在哪里，而不是软件用在哪里），页面里都会写明。

> CLI：`x shodan` —— 见 [x-cmd.com/mod/shodan](https://x-cmd.com/mod/shodan)
> 数据：[api.shodan.io](https://api.shodan.io) · [internetdb.shodan.io](https://internetdb.shodan.io)（免 key）
> facet 清单是活的：`x shodan trend facets`
