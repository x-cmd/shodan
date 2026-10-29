# Where the exposure is

## Two different questions

"Is this product deployed here?" and "is the abuse coming from here?" sound like the same question. They are not, and they have different answers, which is why this page shows both.

- **Product geography** — where the servers running a piece of software physically are. This is a fact about **market structure**: who hosts, where, and at what density.
- **Abuse geography** — where the hosts running a dangerous protocol actually live. This is a fact about **resale and abuse**: which networks carry the traffic that gets abused, and which providers are willing to host it.

## Product geography — nginx by country

```text
https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts" width="780" alt="where-the-exposure-is"></a>

56.9M observable nginx hosts. The distribution is heavily concentrated: the United States, China and Hong Kong alone hold a very large share.

**Hong Kong is the standout and deserves explanation rather than a shrug.** A small territory holding a disproportionate share of exposed servers is not a coincidence about Hong Kong's users — it is a fact about Hong Kong's *hosting* market: an enormous density of small, cheap, frequently-configured servers, many of them running whatever software their operator installed. This is the same shape as the "bulletproof hosting" discussion, and the numbers are consistent with it.

## Abuse geography — telnet and RDP

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts" width="780" alt="where-the-exposure-is"></a>

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts" width="780" alt="where-the-exposure-is"></a>

Telnet exposure is tiny in absolute terms (1.15M hosts) and RDP is around 2.4M. Both are ports that show up repeatedly in breach write-ups, and both are distributed differently from nginx: not by "where people use the protocol" but by "which networks end up carrying it".

The operational consequence is direct. **If you are writing a blocklist, you want the abuse geography. If you are sizing a market or planning a deployment, you want the product geography.** Using one to answer the other is a common and expensive mistake.

## A note on what Shodan's country data can and cannot tell you

- **`country` is the IP's geolocation**, which for cloud and CDN ranges is the location of the *allocation*, not of the operator. A Cloudflare range is registered in one country and serves the world.
- **Counts are Shodan-observable**, so countries with dense scanning coverage are over-represented.
- **Neither cut tells you anything about the people using the service**, which is almost never what you want to know anyway.

> Reproduce: `x shodan host count -f country 'product:nginx'` — note the argument order;
> `-f` must come **before** the query. (This ordering is a bug in the current module — see x-cmd/x-cmd#515.)
