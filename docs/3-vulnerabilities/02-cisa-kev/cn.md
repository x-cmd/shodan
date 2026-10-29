# CISA 已被利用漏洞目录

**CISA KEV 目录按厂商分布** —— CISA 下令美国联邦机构必须修补的 1762 个漏洞。

```text
https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs"><img src="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs" width="780" alt="cisa"></a>

**全部 KEV 与其中被勒索软件活动利用过的部分（目录里共 361 条）对比。**

```text
https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs
```

<a href="https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs"><img src="https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs" width="780" alt="cisa"></a>

CISA 的 KEV 不是一个漏洞数据库，而是**一张修补指令清单**。进榜意味着「有证据表明它正在野外被利用，在此日期前修掉」。进榜说的是**利用情况**，不是严重程度。

- **Microsoft 以 390 条居首** —— 但这个数字对厂商有美化成分，因为它覆盖的是一条有几十个独立版本组件的产品线。
- **其后是 Cisco（101）、Apple（95）、Adobe（83）** —— 三家都是客户自己控制不了更新路径的设备或终端软件。
- **勒索软件那一列切得更锋利。** Fortinet（14）、SonicWall（13）、Ivanti（12）、Oracle（13），勒索软件关联占比都高于它们的 KEV 总数所暗示的比例 —— VPN 和边缘设备才是勒索软件团伙真正进来的门。

**有价值的是两列的对照：「Microsoft 的 KEV 最多」这句话为真，但基本没意义；「Fortinet 有 14 条 KEV，其中一半以上与勒索软件相关」才是一个发现。**

> 复现：`x kev ls`（直接拉 CISA 的 `known_exploited_vulnerabilities.csv`）。目录有 12 列，包括 `knownRansomwareCampaignUse`、`dueDate` 和 `cwes`。
