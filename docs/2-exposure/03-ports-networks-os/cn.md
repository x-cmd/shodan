# 端口、网络与操作系统

## 端口的长尾本身就是指纹

端口分布通常被认为没什么可看，当然是 80 和 443 占主导。有意思的是尾巴。

```text
https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts" width="780" alt=""></a>

在 80 和 443 后面，是一长串替代 HTTP 端口：888、5000、5001、2052、2082、2083、2086、2087、2095、2096、8080、8443、8880。

这些不是随便挑的。8880 和 8443 是特定面板和代理栈的默认值。nginx 前面挂一个非标准 HTTP 端口，通常是三件事之一：一个管理面板、一条隧道或代理，或者刻意避开默认端口。无论哪种，端口号本身就告诉你在连上去之前那套栈是什么。

这就是把端口画成图而不是列成表的实际价值：端口分布是一群人部署习惯的指纹，而部署习惯直接对应攻击面。

## 哪些网络在承载这些暴露

```text
https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts" width="780" alt=""></a>

```text
https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts" width="780" alt=""></a>

ASN 那一刀回答的是问题的另一半：不是暴露了什么，而是谁在暴露。

445 端口上的 SMB 是最刺眼的一个。它是互联网上后果最严重的暴露端口，绝大多数勒索软件和蠕虫 campaigns 的骨干，而它的暴露高度集中在少数几家大型托管服务商身上。这些服务商也出现在每一份滥用报告的榜首。这是供应链层面的观察，不是巧合。

nginx 的 ASN 分布则更分散，这符合 nginx 的角色：一个谁都在用的通用服务器。

## 外面跑着什么操作系统

```text
https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts" width="780" alt=""></a>

HTTPS 主机的 OS 分布。Windows Server 的占比比很多人预期的小，这反映了过去十年的容器化：443 上暴露的东西大多是 Linux，而且其中相当一部分 Linux 其实是容器镜像而不是一台主机。

## 这三个切法合起来对资产盘点有什么用

把三刀放在一起，你得到的画面用别的方式确实很难拿到。端口那一刀告诉你主机上跑的是哪套栈（管理面板？代理？普通 web 服务器？），ASN 那一刀告诉你面对的是哪个运营者，OS 那一刀告诉你技术上在处理什么。

真正有意思的产出是它们的交叉。某一个 ASN 里大量 SMB 暴露，是关于那家服务商客户群的故事；跨很多 ASN 的 8880 端口，则是一个软件面板默认配置的故事。两者都只有在看交集时才看得见。

## 注意事项

端口和产品都是 Shodan 观测到的，藏在代理后面、或配置非标准的服务，不会按你以为的方式归属。端口 facet 返回的是按数量排的前五名而不是完整分布，前五之外的尾巴在这里是看不见的。ASN 计数被大型服务商主导，而真正有意思的例外恰恰被切掉了。

> 复现：`x shodan host count -f port 'product:nginx'`，`-f asn` 和 `-f os` 同理。
> 注意 `-f` 要放在查询前面，见 x-cmd/x-cmd#515。
