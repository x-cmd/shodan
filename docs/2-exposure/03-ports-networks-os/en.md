# Ports, networks, operating systems

## The port long tail is a fingerprint

Port distributions are usually dismissed as boring — of course 80 and 443 dominate. The interesting part is the tail.

```text
https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts" width="780" alt="ports-networks-operating-sys"></a>

Behind 80 and 443 there is a long list of **alternative HTTP ports**: 888, 5000, 5001, 2052, 2082, 2083, 2086, 2087, 2095, 2096, 8080, 8443, 8880.

These are not random choices. **8880 and 8443 are the defaults of specific panels and proxy stacks.** A non-standard HTTP port in front of nginx almost always means one of three things: a management panel, a tunnel/proxy, or a deliberate attempt to stay off the default ports. Either way, the port number alone tells you what stack is running **before you connect to it**.

This is the practical value of charting ports rather than listing them: a port distribution is a **fingerprint of the deployment practices of a population**, and deployment practices map to attack surface.

## Which networks carry the exposure

```text
https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=as54994%2Cas16509%2Cas37963%2Cas14061%2Cas4837&values=17186485%2C2988458%2C2160738%2C1412087%2C1329742&title=Internet-exposed%20nginx%20by%20ASN&unit=hosts" width="780" alt="ports-networks-operating-sys"></a>

```text
https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=as17557%2Cas139341%2Cas16276%2Cas40509%2Cas396982&values=92909%2C45540%2C30744%2C27459%2C27345&title=Internet-exposed%20SMB%20%28port%3A445%29%20by%20ASN&unit=hosts" width="780" alt="ports-networks-operating-sys"></a>

The ASN view answers the other half of the question: not *what* is exposed, but *who* is exposing it.

**SMB (port 445) is the striking one.** It is the most consequential exposed port on the internet — the backbone of most ransomware and worm campaigns — and its exposure concentrates hard in a small number of large hosting providers. Those same providers appear at the top of every abuse report. This is a supply-chain observation, not a coincidence.

The nginx ASN distribution is broader and more diffuse, which matches what nginx is: a general-purpose server run by everyone.

## What operating systems are out there

```text
https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=Ubuntu%2CWindows%2CLinux%203.x%2CWindows%207%20or%208%2CLinux%202.6.x&values=2077679%2C1149468%2C651459%2C435006%2C396956&title=OS%20of%20internet-exposed%20HTTPS%20hosts&unit=hosts" width="780" alt="ports-networks-operating-sys"></a>

HTTPS hosts by OS. Windows Server has a smaller share than you might expect, which reflects the last decade of containerisation — most of what is exposed on 443 is Linux, and a lot of that Linux is a container image rather than a host.

## Why this combination is useful for asset inventory

Take the three cuts together and you get a picture that is genuinely harder to get any other way:

- **The port cut** tells you which *stack* a host is running (a management panel, a proxy, a plain web server).
- **The ASN cut** tells you which *operator* you are dealing with.
- **The OS cut** tells you what you are dealing with technically.

The interesting output is the **cross-product**. A large population of SMB exposure in one ASN is a story about that provider's customers. A population of port 8880 across many ASNs is a story about a software panel's default configuration. Both are visible only when you look at the intersection.

## Caveats

- Ports and products are Shodan-observed; a service behind a proxy or a non-standard configuration will not be attributed the way you would guess.
- The port facet returns the top five by count, not the full distribution. A long tail beyond the top five is invisible here.
- ASN counts are dominated by large providers; the interesting exceptions are exactly what gets cut off.

> Reproduce: `x shodan host count -f port 'product:nginx'` · `-f asn` · `-f os`.
> Note that `-f` must precede the query — see x-cmd/x-cmd#515.
