# 暴露面在哪里

**互联网暴露的 nginx，按国家。**

```text
https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts" width="780" alt=""></a>

**互联网暴露的 telnet（23 端口），按国家。**

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts" width="780" alt=""></a>

**互联网暴露的 RDP（3389 端口），按国家。**

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts" width="780" alt=""></a>

地理是第一个问题，而答案比总量看起来更偏：

- **nginx 的暴露高度集中在少数几个国家** —— 美国、中国和香港分走了 5690 万暴露 nginx 主机里很大一块。香港是最突出的：地方不大，暴露服务器占比却极高 —— 这是托管市场的结构性事实，不是巧合。
- **telnet 和 RDP 完全是另一种形状。** telnet 的绝对暴露量很小（115 万台），且高度偏向少数几个网络；RDP 约 240 万台。这两个端口正是泄露报告里反复出现的，而它们的分布反映的是*哪些供应商在转售接入*，不是*哪些国家在用这个协议*。

这个区分很重要：**「产品在哪里」和「滥用在哪里」是两个问题，两个答案。**

> 复现：`x shodan host count -f country 'product:nginx'` —— 注意参数顺序，`-f` 必须放在查询**前面**。
