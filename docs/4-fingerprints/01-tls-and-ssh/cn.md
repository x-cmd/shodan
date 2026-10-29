# TLS 与 SSH 指纹

## 证书的问题出在哪

TLS 证书告诉你**是谁在出示**。它不告诉你**是什么在终结连接**。这是两个不同的问题，而在今天的互联网上，它们的答案经常不一样。

一台通过 CDN 提供服务的机器，出示的是 CDN 的证书 —— 一张完全合法、正确签发、对应你请求的那个域名的证书。而在它后面可能是 nginx、Apache、一个 Go 程序、一个 Python Flask 开发服务器，或者一台用默认 TLS 栈跑了四年的设备。这几种情况下证书一模一样。

要弄清后面到底有什么，只能给握手过程本身做指纹。

## JARM：给 TLS 协议栈做指纹

```text
https://x-cmd.com/draw?type=hbar&labels=00000000000000000000000000000000000000000000000000000000000000%2C29d29d00029d29d00041d41d00041dcb3f8752a2d70d1dbf446fc46ab5df96%2C29d29d00029d29d21c29d29d29d29dab965d4179316d1f3a3c5dbe7d8829db%2C29d29d00029d29d00029d29d29d29da8f16e8f0a21d89b06fef5080cbd1b2a%2C29d29d00029d29d21c41d41d00041d0fc7ac8335432249e8becb757baaacec&values=9716808%2C4035455%2C2432295%2C2148551%2C1712644&title=TLS%20JARM%20fingerprint%20share%20%28port%3A443%29&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=00000000000000000000000000000000000000000000000000000000000000%2C29d29d00029d29d00041d41d00041dcb3f8752a2d70d1dbf446fc46ab5df96%2C29d29d00029d29d21c29d29d29d29dab965d4179316d1f3a3c5dbe7d8829db%2C29d29d00029d29d00029d29d29d29da8f16e8f0a21d89b06fef5080cbd1b2a%2C29d29d00029d29d21c41d41d00041d0fc7ac8335432249e8becb757baaacec&values=9716808%2C4035455%2C2432295%2C2148551%2C1712644&title=TLS%20JARM%20fingerprint%20share%20%28port%3A443%29&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=00000000000000000000000000000000000000000000000000000000000000%2C29d29d00029d29d00041d41d00041dcb3f8752a2d70d1dbf446fc46ab5df96%2C29d29d00029d29d21c29d29d29d29dab965d4179316d1f3a3c5dbe7d8829db%2C29d29d00029d29d00029d29d29d29da8f16e8f0a21d89b06fef5080cbd1b2a%2C29d29d00029d29d21c41d41d00041d0fc7ac8335432249e8becb757baaacec&values=9716808%2C4035455%2C2432295%2C2148551%2C1712644&title=TLS%20JARM%20fingerprint%20share%20%28port%3A443%29&unit=hosts" width="780" alt="tls-ssh"></a>

JARM 是一种**主动**指纹 —— 它靠发送一个故意畸形的 TLS ClientHello，然后观察服务器的回应来工作。回应模式（提供哪些密码套件、顺序如何、出现什么扩展、错误怎么格式化）是服务器 TLS 实现的函数。两个跑着不同协议栈的服务器，即使通过完全相同的证书提供完全相同的内容，也会产生不同的 JARM 值。

这里的分布显示出它有多集中：少数几个指纹覆盖了数百万台主机。每一个指纹就是一个大规模部署的、不同的 TLS 实现，而每一个都有自己的怪癖、自己的 bug、自己的缓解措施。

**这在实际中为什么重要：**
- 你能数出「一个表面服务」背后实际存在多少种实现。
- 每种实现有一组不同的已知 CVE，所以指纹告诉你哪些公告适用。
- 指纹与自己声称的技术栈对不上的主机，值得单独查。

还有一个相关 facet `ssl.ja3s`，它指纹的是**客户端**一侧 —— 对识别"是什么软件在连你"很有用。

## SSH 密钥指纹：有多少人共用一把钥匙

```text
https://x-cmd.com/draw?type=hbar&labels=2f%3A1c%3A34%3Ac9%3A4c%3A56%3A12%3A6c%3Ace%3Af2%3A10%3Aee%3A0f%3A3e%3A41%3Afe%2Cd8%3A0e%3A4d%3Ad3%3Ad7%3A00%3Ad7%3Aaa%3A8f%3A69%3A9f%3Aec%3Aaf%3A90%3Ab3%3A16%2C78%3Ae3%3Ac6%3Ad0%3A2d%3A4c%3Af8%3A3e%3A87%3A8c%3Abf%3A34%3Ad6%3A72%3A6f%3A46%2C8b%3A75%3A88%3A08%3A41%3A78%3A11%3A5b%3A49%3A68%3A11%3A42%3A64%3A12%3A6d%3A49%2C3e%3A75%3A5f%3A41%3A69%3Ad1%3A67%3A42%3A5e%3Ad1%3A03%3Adb%3Ac8%3A04%3A90%3A84&values=142337%2C39179%2C34007%2C27799%2C21855&title=SSH%20keys%20reused%20across%20the%20internet%20%28port%3A22%29&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=2f%3A1c%3A34%3Ac9%3A4c%3A56%3A12%3A6c%3Ace%3Af2%3A10%3Aee%3A0f%3A3e%3A41%3Afe%2Cd8%3A0e%3A4d%3Ad3%3Ad7%3A00%3Ad7%3Aaa%3A8f%3A69%3A9f%3Aec%3Aaf%3A90%3Ab3%3A16%2C78%3Ae3%3Ac6%3Ad0%3A2d%3A4c%3Af8%3A3e%3A87%3A8c%3Abf%3A34%3Ad6%3A72%3A6f%3A46%2C8b%3A75%3A88%3A08%3A41%3A78%3A11%3A5b%3A49%3A68%3A11%3A42%3A64%3A12%3A6d%3A49%2C3e%3A75%3A5f%3A41%3A69%3Ad1%3A67%3A42%3A5e%3Ad1%3A03%3Adb%3Ac8%3A04%3A90%3A84&values=142337%2C39179%2C34007%2C27799%2C21855&title=SSH%20keys%20reused%20across%20the%20internet%20%28port%3A22%29&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=2f%3A1c%3A34%3Ac9%3A4c%3A56%3A12%3A6c%3Ace%3Af2%3A10%3Aee%3A0f%3A3e%3A41%3Afe%2Cd8%3A0e%3A4d%3Ad3%3Ad7%3A00%3Ad7%3Aaa%3A8f%3A69%3A9f%3Aec%3Aaf%3A90%3Ab3%3A16%2C78%3Ae3%3Ac6%3Ad0%3A2d%3A4c%3Af8%3A3e%3A87%3A8c%3Abf%3A34%3Ad6%3A72%3A6f%3A46%2C8b%3A75%3A88%3A08%3A41%3A78%3A11%3A5b%3A49%3A68%3A11%3A42%3A64%3A12%3A6d%3A49%2C3e%3A75%3A5f%3A41%3A69%3Ad1%3A67%3A42%3A5e%3Ad1%3A03%3Adb%3Ac8%3A04%3A90%3A84&values=142337%2C39179%2C34007%2C27799%2C21855&title=SSH%20keys%20reused%20across%20the%20internet%20%28port%3A22%29&unit=hosts" width="780" alt="tls-ssh"></a>

`ssh.fingerprint` 这个 facet 按**SSH 服务端提供的公钥**给主机计数。少数几个指纹主导了分布，意味着大量主机共用同一把私钥。

这有两层实打实的重要性：

**这是一个安全问题。** 共用私钥意味着一次泄露 —— 一份备份、一个退役的机器、一块被取走的硬盘 —— 就同时打开成千上万台主机。它也意味着这些主机在攻击者和防守者眼里完全无法区分。

**它是一个指纹工具。** 提供广泛共用密钥的主机极易被识别，于是这个群体可以被清点和追踪。

## 为什么这两刀对资产盘点都重要

把两者放在一起，它们把一份主机列表变成一份**实现清单**：

- **TLS 指纹**说明是什么栈在终结连接。
- **SSH 指纹**说明这台主机用的是哪把密钥，因此属于哪个群体。

两台主机从外面看可以完全一样 —— 同样的端口、同样的 banner、同样的证书 —— 而在这两件事上却不同。多数资产清单停在证书这一步。这两个是接下来的两个问题。

## 注意事项

- 指纹识别的是*实现*，不是*版本*。打了补丁和没打补丁的同一个栈，共享同一个指纹。
- JARM 的值取决于库版本及其配置；有些栈允许通过配置改变指纹。
- 指纹分布受"哪些栈流行"的强烈影响，而不是"哪些栈安全"。

> 复现：`x shodan host count -f ssl.jarm 'port:443'` ·
> `x shodan host count -f ssh.fingerprint 'port:22'`。
> （记住：`-f` 要放在查询前面 —— 见 x-cmd/x-cmd#515。）
