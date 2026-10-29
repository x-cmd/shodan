# Where the exposure is

**Internet-exposed nginx by country.**

```text
https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts" width="780" alt="where-the-exposure-is"></a>

**Internet-exposed telnet (port 23) by country.**

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CBR%2CUS%2CKR%2CJP&values=408643%2C134908%2C64871%2C39415%2C37486&title=Internet-exposed%20telnet%20%28port%3A23%29%20by%20country&unit=hosts" width="780" alt="where-the-exposure-is"></a>

**Internet-exposed RDP (port 3389) by country.**

```text
https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CN%2CUS%2CSG%2CDE%2CGB&values=789547%2C426853%2C202957%2C132330%2C92599&title=Internet-exposed%20RDP%20%28port%3A3389%29%20by%20country&unit=hosts" width="780" alt="where-the-exposure-is"></a>

Geography is the first question, and the answers are more lopsided than the totals suggest:

- **nginx exposure is concentrated in a handful of countries** — the United States, China and Hong Kong hold a large share of the 56.9M exposed nginx hosts. Hong Kong is the standout: it is a small place with a very large share of exposed servers, which is a hosting-market fact, not a coincidence.
- **telnet and RDP are a different shape entirely.** Telnet's exposure is tiny in absolute terms (1.15M hosts) and heavily skewed toward a few networks; RDP is around 2.4M. These are the ports that show up in breach reports, and their distribution is about *which providers resell access*, not about which countries use the protocol.

The distinction matters: **"where is the product"** and **"where is the abuse"** are different questions with different answers.

> Reproduce: `x shodan host count -f country 'product:nginx'` — note the argument order; `-f` must come *before* the query.
