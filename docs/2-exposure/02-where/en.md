# Where the exposure is

## Two different questions

"Is this product deployed here?" and "is the abuse coming from here?" sound like the same question. They are not, and they have different answers, which is why this page shows both.

Product geography says where the servers running a piece of software physically are, which is a fact about market structure: who hosts, where, and at what density. Abuse geography says where the hosts running a dangerous protocol actually live, which is a fact about resale and abuse: which networks carry the traffic that gets abused, and which providers are willing to host it.

## Product geography: nginx by country

```text
https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts" width="780" alt="where-the-exposure-is"></a>

56.9 million observable nginx hosts, and a heavily concentrated distribution: the United States, China and Hong Kong alone hold a very large share.

Hong Kong is the standout and deserves an explanation rather than a shrug. A small territory holding a disproportionate number of exposed servers is not a coincidence about Hong Kong's users, it is a fact about Hong Kong's hosting market: an enormous density of small, cheap, frequently-configured servers, many running whatever software their operator installed. This is the same shape as the "bulletproof hosting" conversation, and the numbers are consistent with it.

## Abuse geography: telnet and RDP

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts" width="780" alt="where-the-exposure-is"></a>

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts" width="780" alt="where-the-exposure-is"></a>

Telnet's exposure is tiny in absolute terms, 1.15 million hosts, and RDP is around 2.4 million. Both are ports that turn up repeatedly in breach write-ups, and both are distributed differently from nginx: not by where people use the protocol but by which networks end up carrying it.

The operational consequence is direct. If you are writing a blocklist you want the abuse geography; if you are sizing a market or planning a deployment you want the product geography. Using one to answer the other is a common and expensive mistake.

## What Shodan's country data can and cannot tell you

`country` is the geolocation of the IP, which for cloud and CDN ranges is the location of the allocation rather than of the operator. A Cloudflare range is registered in one country and serves the world. The counts are Shodan-observable, so countries with dense scanning coverage are over-represented. And neither cut tells you anything about the people using the service, which is almost never what you want to know anyway.

> Reproduce: `x shodan host count -f country 'product:nginx'`, and note the argument order: `-f` comes before the query. That ordering is a bug in the current module, tracked at x-cmd/x-cmd#515.
