# 端口、网络与操作系统

**互联网暴露的 nginx，按端口。**

```text
https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts" width="780" alt=""></a>

**互联网暴露的 nginx，按 ASN。**

```text
https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts" width="780" alt=""></a>

**互联网暴露的 SMB（445 端口），按 ASN。**

```text
https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts" width="780" alt=""></a>

**互联网暴露的 HTTPS 主机的操作系统分布。**

```text
https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts" width="780" alt=""></a>

nginx 的端口分布是最有意思的一张，因为它并不是人们预期的那份端口清单：

- **80 和 443 占绝对主导** —— 但后面拖出一条「非标准 HTTP 端口」的长尾：888、5000、5001、2052、2082、2083、2086、2087、2095、2096、8080、8443、8880。这些是 nginx 被**刻意**部署在非标准端口上的地方，通常藏在隧道或某个管理面板后面。
- **这条尾巴本身就是指纹。** 8880 和 8443 不是「有人随手挑了个端口」—— 它们是特定面板和代理栈的默认值。连上之前，光看端口就知道跑的是什么软件。

ASN 那张图回答的是另一半：**这些暴露来自哪些网络**。SMB 的暴露尤其集中在大 hosting 服务商身上 —— 和每一份滥用报告里排在最前面的，是同一批。

> 复现：`x shodan host count -f port 'product:nginx'` · `-f asn` · `-f os`。
