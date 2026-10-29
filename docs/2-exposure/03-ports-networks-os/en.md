# Ports, networks, operating systems

**Internet-exposed nginx by port.**

```text
https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts" width="780" alt="ports-networks-operating-sys"></a>

**Internet-exposed nginx by ASN.**

```text
https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts" width="780" alt="ports-networks-operating-sys"></a>

**Internet-exposed SMB (port 445) by ASN.**

```text
https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts" width="780" alt="ports-networks-operating-sys"></a>

**OS of internet-exposed HTTPS hosts.**

```text
https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts" width="780" alt="ports-networks-operating-sys"></a>

The port breakdown of exposed nginx is the interesting one, because it is not the port list anyone expects:

- **Port 80 and 443 dominate** — but then a long tail of the "alternative HTTP" ports appears: 888, 5000, 5001, 2052, 2082, 2083, 2086, 2087, 2095, 2096, 8080, 8443, 8880. These are the ports where nginx is deployed deliberately, on a non-standard port, usually behind a tunnel or a panel.
- **That tail is a fingerprint.** Port 8880 and 8443 are not "someone chose a random port" — they are the defaults of specific panels and proxy stacks. A single port tells you what software is running before you connect.

The ASN view answers the other half: **which networks are the sources**. SMB exposure in particular concentrates hard in a few large hosting providers — the same providers that appear at the top of every abuse report.

> Reproduce: `x shodan host count -f port 'product:nginx'` · `-f asn` · `-f os`.
