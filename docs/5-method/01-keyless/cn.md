# 免 key 的那条路

## 上面所有内容都要 key。这条不用。

Shodan 的主 API 需要账号和 API key。但还有第二个端点，`internetdb.shodan.io`，对单个主机返回同样结构的数据，而且**不要 key、不要账号、不用协商限流**：

```sh
curl -s https://internetdb.shodan.io/1.1.1.1
```

```json
{"cpes":["cpe:/a:cloudflare:cloudflare"],
 "hostnames":["one.one.one.one","wtby.wlc.theeyecaregroup.com"],
 "ip":"1.1.1.1",
 "ports":[53,69,80,443,2053,2082,2087,2096,8080,8443,8880],
 "tags":["pqc"],
 "vulns":[]}
```

每个 IP 一次 `curl`。接口就这么多。

## 十台知名主机各自开放的端口数

```text
https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports
```

<a href="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports"><img src="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports" width="780" alt="key"></a>

## 同样这十台主机的端口出现频次

```text
https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts
```

<a href="https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts"><img src="https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts" width="780" alt="key"></a>

## 这组样本说明了什么

两台主机 —— Cloudflare 的 `104.16.0.1`（13 个端口）和 `162.159.1.1`（12 个）—— 带着整套**替代 HTTP 端口**（2052、2082、2083、2086、2087、2095、2096、8080、8443、8880）。这些是 Cloudflare 的 spectrum/代理端口，也正是"我该开哪些端口"这类对话里从来不会被提起的那些。

另一头，`8.8.8.8` 和 `13.107.42.14` 各只开两个端口。从 2 个到 13 个的差距，不是关于这些机构的，而是关于**它们跑的产品**以及**那些产品被定制到什么程度**的。

## 什么时候用哪个

**用 internetdb，当：**
- 你手上拿到了一份 IP 清单 —— 泄露的凭据表、钓鱼目标表、事件响应清单 —— 想快速分诊。这是最快的第一轮，因为完全不需要任何配置。
- 你在写脚本，想要一个没有依赖的检查。
- 你要的是逐主机的答案，不需要聚合。

**用需要鉴权的 API，当：**
- 你需要**聚合** —— 这正是这个站存在的意义。facet 分解需要带 query 和 facets 参数的 `/shodan/host/count`。
- 你需要趋势历史（113 个月）或扫描/流式端点。
- 你需要确切的计数，而不是抽样。

## 自己写一个聚合脚本

因为这个端点足够简单，「一份 IP 清单的暴露图景」大约十五行就够：

```sh
for ip in $(cat ips.txt); do
  curl -s "https://internetdb.shodan.io/$ip"
done | jq -r '.ports[]?' | sort | uniq -c | sort -rn
```

再配上 `jq` 的分组，你就有了一份任意地址集合的端口直方图 —— 一家新供应商的地址段、一个客户的资产、或者一份泄露清单 —— 不需要账号，几秒钟的事。

## 注意事项

- internetdb 覆盖的是 Shodan 索引的**一个子集** —— 它为最常被查询的字段做了优化，不提供 facet、趋势和扫描。
- 一台不在 Shodan 索引里的主机返回 `{"detail":"Not Found"}`，意思是"我们没有它的信息"，**不是**"这台机器挂了"。这是两个完全不同的断言。
- 数据是 Shodan 上次扫到那台主机时的快照，可能已是几个月前的。

> 复现：拿任意 IP 清单循环 `curl -s https://internetdb.shodan.io/<ip>`，把 `ports` 数组聚合起来。
