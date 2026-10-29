# CISA 已被利用漏洞目录

## KEV 到底是个什么东西

CISA 的「已被利用漏洞」(KEV) 目录不是漏洞数据库，也不是严重性榜单。它是一张**修补指令单**。进榜的意思是：*我们有证据表明它正在野外被利用，美国联邦机构必须在此日期前修掉。*

进榜说的是**利用情况**，不是这个 bug 有多坏。有些 KEV 是 CVSS 4.0 的漏洞；有些则纯粹是技术债，只是因为别的东西都修好了它才变得可利用。目录里有 1762 条。

## 按厂商

```text
https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs"><img src="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs" width="780" alt="cisa"></a>

## 按厂商，并按勒索软件切分

```text
https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs
```

<a href="https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs"><img src="https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs" width="780" alt="cisa"></a>

1762 条里有 361 条的 `knownRansomwareCampaignUse = Known`。

## 读第一张图

**Microsoft 以 390 条领先** —— 而这个数字其实美化了它。Microsoft 出品几十个各自独立版本化的组件（Windows、Exchange、SharePoint、Office、Azure 各自的补丁节奏），每一个都会自己开一堆 CVE。产品面越大的厂商，按厂商计数必然排在前面。后面是 Cisco（101）、Apple（95）、Adobe（83）—— 这三家都是设备或终端软件，用户自己控制不了更新路径。

## 读第二张图 —— 更锋利的那一刀

有意思的不是"谁 KEV 最多"，而是**"谁的暴露正在被主动变现"**：

- **Fortinet：14 条 KEV，其中一半以上与勒索软件相关。**
- **SonicWall：13 条，勒索软件占比很高。**
- **Ivanti：12 条，多数与勒索软件相关。**
- **Oracle：13 条。**

这些是 VPN 集中器、防火墙和边缘设备。**这恰恰是勒索软件团伙进来的那一类设备** —— 面向互联网、带着凭据、而且经常因为更新要走过支持合同而没打上补丁。

**"Microsoft 的 KEV 最多"这句话为真，但基本没意思；"Fortinet 有 14 条 KEV，其中一半以上与勒索软件相关"才是一个发现。**

## 关键在于把两张图并排读

两张图放在一起，一个具体得多的说法就成立了：

> KEV 身份告诉你这个产品**是否**正被利用。勒索软件那一列告诉你这种利用**如何变现**。对边缘设备来说这两者塌缩成同一件事，对消费级软件来说则完全是两回事。

在第一列而不在第二列的产品，是被拿来做入侵、间谍或僵尸网络的。两列都在的，是被拿来做钱的。

## 为什么这值得单独一页

KEV 目录是公开的、免费的、每天更新的 —— 而对绝大多数人来说，它完全没人读。它是一个 12 列的 CSV。把它渲染成一张图就是全部意义所在：**一份没人读的监管指令单，变成一张网络团队能在会上照着做的图。**

同样的图对你自己组织的漏洞管理也管用 —— 把你的补丁积压映射到同样这两列上就行。

## 注意事项

- KEV 目录是**美国联邦范围**的。它的内容反映的是美国机构被告知的情况，不是全球利用情况的普查。
- "与勒索软件相关"指的是 CISA 对那个 campaign 有归因，这不等同于"它只被这么利用"。
- 按厂商的计数反映的是产品面大小，不是安全态势。

> 复现：`x kev ls`（直接拉 CISA 的 `known_exploited_vulnerabilities.csv`）。
> 目录有 12 列，包括 `knownRansomwareCampaignUse`、`dueDate`、`requiredAction` 和 `cwes`。
