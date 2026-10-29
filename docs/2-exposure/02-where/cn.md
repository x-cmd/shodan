# 暴露面在哪里

## 两个不同的问题

"这个产品部署在哪些地方？"和"滥用是从哪些地方来的？"听起来像同一个问题。它们不是，答案也不同 —— 所以这一页把两者都摆出来。

- **产品地理** —— 运行某软件的服务器物理上在哪里。这是**市场结构**的事实：谁在托管、在哪里、密度多大。
- **滥用地理** —— 运行危险协议的主机实际住在哪里。这是**转售与滥用**的事实：哪些网络承载了被滥用的流量，哪些服务商愿意托管它们。

## 产品地理 —— nginx 按国家

```text
https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts" width="780" alt=""></a>

5690 万可观测 nginx 主机。分布高度集中：美国、中国和香港本身就占了很大一块。

**香港是最突出的那个，值得解释而不是耸耸肩。** 一个不大的地方持有不成比例的暴露服务器数量，这不是关于香港用户的巧合，而是关于香港**托管**市场的事实：密度极高的、便宜的小服务器，很多都跑着运维随手装上的软件。这和"抗打击托管"（bulletproof hosting）那场讨论的形状是一样的，数字也吻合。

## 滥用地理 —— telnet 与 RDP

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts" width="780" alt=""></a>

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts" width="780" alt=""></a>

telnet 的绝对暴露量很小（115 万台），RDP 约 240 万台。这两个端口在泄露报告里反复出现，而它们的分布方式跟 nginx 完全不同：不是"哪里的人在用这个协议"，而是"哪些网络最终承载着它"。

操作上的结论很直接。**如果你在写封禁名单，你要的是滥用地理；如果你在估市场规模或规划部署，你要的是产品地理。** 拿一个去回答另一个，是常见且代价高昂的错误。

## 关于 Shodan 国别数据能说明什么、不能说明什么

- **`country` 是 IP 的地理位置**，对云和 CDN 的地址段来说，那是*分配*的位置，不是运营者的位置。一个 Cloudflare 地址段注册在一个国家，服务全世界。
- **计数是 Shodan 可观测到的**，所以扫描覆盖密的国家被高估了。
- **两种切法都不告诉你服务的使用者是谁** —— 而那几乎永远不是你真正想知道的。

> 复现：`x shodan host count -f country 'product:nginx'` —— 注意参数顺序，
> `-f` 必须放在查询**前面**。（这个顺序目前是模块的 bug，见 x-cmd/x-cmd#515。）
