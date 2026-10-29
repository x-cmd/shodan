# 免 key 的那条路

上面所有内容用的是需要鉴权的 API。还有第二条路 —— 一个**免 key** 的端点，对单个主机返回同样的结构：

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

**十台知名主机各自开放的端口数。**

```text
https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports
```

<a href="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports"><img src="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports" width="780" alt="key"></a>

**同样这十台主机的端口出现频次。**

```text
https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts
```

<a href="https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts"><img src="https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts" width="780" alt="key"></a>

不需要 API key、不需要账号、不用协商限流 —— 每个 IP 一次 `curl`。所以它是**拿到一份 IP 清单后先看一眼**的合适工具：泄露的凭据表、钓鱼目标表、事件响应里要排查的地址列表。

这十台主机的样本，恰好是现代边缘的一个公允缩影：Cloudflare 的 `104.16.0.1`（13 个端口）和 `162.159.1.1`（12 个）带着完整的那套「替代 HTTP 端口」，而 `8.8.8.8` 和 `13.107.42.14` 各只开两个。

> 复现：拿任意 IP 列表循环 `curl -s https://internetdb.shodan.io/<ip>`，把 `ports` 数组聚合起来。
