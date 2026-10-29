# shodan

**The exposed internet, as plain URLs.**

Shodan indexes what is *running* on the internet rather than what pages *say*, and it breaks any search down by 39 facets — country, port, ASN, OS, TLS fingerprint, SSH key, CVE, WAF. Those breakdowns arrive as JSON, and JSON in an issue or a blog post is something people scroll past.

This site renders them. Because a chart here is a URL with the parameters written out, the query is editable: point it at `org:yourcompany` instead of `product:nginx` and the same template becomes a picture of your own attack surface.

> Languages: English · 中文（每页双语；es / ja / de 待补）

## Sneak peek

### Service exposure over time — [full page](docs/2-exposure/01-trends/en.md)

<a href="https://x-cmd.com/draw?type=line&labels=21-11%2C21-12%2C22-01%2C22-02%2C22-03%2C22-04%2C22-05%2C22-06%2C22-07%2C22-08%2C22-09%2C22-10%2C22-11%2C22-12%2C23-01%2C23-02%2C23-03%2C23-04%2C23-05%2C23-06%2C23-07%2C23-08%2C23-09%2C23-10%2C23-11%2C23-12%2C24-01%2C24-02%2C24-03%2C24-04%2C24-05%2C24-06%2C24-07%2C24-08%2C24-09%2C24-10%2C24-11%2C24-12%2C25-01%2C25-02%2C25-03%2C25-04%2C25-05%2C25-06%2C25-07%2C25-08%2C25-09%2C25-10%2C25-11%2C25-12%2C26-01%2C26-02%2C26-03%2C26-04%2C26-05%2C26-06%2C26-07%2C26-08%2C26-09%2C26-10&series=nginx%3A25672889%2C26201167%2C26589219%2C26533100%2C27588784%2C27378939%2C27411974%2C27251911%2C27531147%2C27437822%2C26997984%2C26459164%2C26505647%2C27028252%2C26411568%2C25070023%2C25733428%2C25456142%2C36164864%2C35115350%2C27690542%2C1737946%2C19820113%2C31602967%2C32292218%2C32232152%2C31961675%2C31095073%2C30863215%2C29356301%2C30099533%2C31247041%2C31218095%2C32713994%2C32911491%2C33946084%2C35021777%2C38544053%2C38498188%2C38096722%2C41349113%2C37537208%2C37120936%2C40749706%2C53221881%2C51499032%2C53733043%2C49116587%2C51527121%2C48320338%2C48569208%2C43642278%2C51553298%2C52810166%2C51794423%2C47658181%2C24750344%2C51153431%2C49292832%2C47492291&series=apache%3A21421035%2C21387365%2C21388556%2C20953769%2C20995495%2C20652798%2C20487143%2C20245492%2C20605137%2C20330808%2C19903496%2C19642338%2C19764958%2C19420665%2C18878105%2C18447751%2C18309726%2C17803215%2C17728950%2C16999300%2C17001133%2C17215648%2C14800271%2C16725307%2C16633667%2C16311843%2C16038676%2C15681202%2C15287459%2C14598388%2C14757081%2C15062796%2C14579108%2C14422186%2C14325475%2C14369709%2C14373511%2C14195948%2C13975488%2C13394819%2C13416010%2C11969503%2C11895539%2C11941092%2C12368796%2C12005996%2C11684668%2C11008748%2C11764180%2C11672128%2C11527857%2C10815629%2C11459745%2C10893290%2C10633716%2C10839517%2C10317442%2C10215785%2C9925894%2C9578480&title=Internet-exposed%20nginx%20vs%20apache%2C%205%20years%20%28Shodan%20Trends%29&unit=hosts"><img src="https://x-cmd.com/draw?type=line&labels=21-11%2C21-12%2C22-01%2C22-02%2C22-03%2C22-04%2C22-05%2C22-06%2C22-07%2C22-08%2C22-09%2C22-10%2C22-11%2C22-12%2C23-01%2C23-02%2C23-03%2C23-04%2C23-05%2C23-06%2C23-07%2C23-08%2C23-09%2C23-10%2C23-11%2C23-12%2C24-01%2C24-02%2C24-03%2C24-04%2C24-05%2C24-06%2C24-07%2C24-08%2C24-09%2C24-10%2C24-11%2C24-12%2C25-01%2C25-02%2C25-03%2C25-04%2C25-05%2C25-06%2C25-07%2C25-08%2C25-09%2C25-10%2C25-11%2C25-12%2C26-01%2C26-02%2C26-03%2C26-04%2C26-05%2C26-06%2C26-07%2C26-08%2C26-09%2C26-10&series=nginx%3A25672889%2C26201167%2C26589219%2C26533100%2C27588784%2C27378939%2C27411974%2C27251911%2C27531147%2C27437822%2C26997984%2C26459164%2C26505647%2C27028252%2C26411568%2C25070023%2C25733428%2C25456142%2C36164864%2C35115350%2C27690542%2C1737946%2C19820113%2C31602967%2C32292218%2C32232152%2C31961675%2C31095073%2C30863215%2C29356301%2C30099533%2C31247041%2C31218095%2C32713994%2C32911491%2C33946084%2C35021777%2C38544053%2C38498188%2C38096722%2C41349113%2C37537208%2C37120936%2C40749706%2C53221881%2C51499032%2C53733043%2C49116587%2C51527121%2C48320338%2C48569208%2C43642278%2C51553298%2C52810166%2C51794423%2C47658181%2C24750344%2C51153431%2C49292832%2C47492291&series=apache%3A21421035%2C21387365%2C21388556%2C20953769%2C20995495%2C20652798%2C20487143%2C20245492%2C20605137%2C20330808%2C19903496%2C19642338%2C19764958%2C19420665%2C18878105%2C18447751%2C18309726%2C17803215%2C17728950%2C16999300%2C17001133%2C17215648%2C14800271%2C16725307%2C16633667%2C16311843%2C16038676%2C15681202%2C15287459%2C14598388%2C14757081%2C15062796%2C14579108%2C14422186%2C14325475%2C14369709%2C14373511%2C14195948%2C13975488%2C13394819%2C13416010%2C11969503%2C11895539%2C11941092%2C12368796%2C12005996%2C11684668%2C11008748%2C11764180%2C11672128%2C11527857%2C10815629%2C11459745%2C10893290%2C10633716%2C10839517%2C10317442%2C10215785%2C9925894%2C9578480&title=Internet-exposed%20nginx%20vs%20apache%2C%205%20years%20%28Shodan%20Trends%29&unit=hosts" width="680" alt="service-exposure-over-time"></a>

### Where the exposure is — [full page](docs/2-exposure/02-where/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=US%2CCN%2CHK%2CJP%2CDE&values=15196466%2C9905515%2C3917034%2C3172078%2C2862913&title=Internet-exposed%20nginx%20by%20country&unit=hosts" width="680" alt="where-the-exposure-is"></a>

### Ports, networks, operating systems — [full page](docs/2-exposure/03-ports-networks-os/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=80%2C443%2C888%2C5000%2C5001&values=11430376%2C10007657%2C465184%2C453353%2C435349&title=Internet-exposed%20nginx%20by%20port&unit=hosts" width="680" alt="ports-networks-operating-systems"></a>

### Known CVEs still exposed — [full page](docs/3-vulnerabilities/01-cve-stock/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=CVE-2018-15473%2CCVE-2023-44487%2CCVE-2022-0778%2CCVE-2017-5638%2CCVE-2014-0160%2CCVE-2019-0708%2CCVE-2020-1472%2CCVE-2021-26855%2CCVE-2022-22965%2CCVE-2023-34362%2CCVE-2021-44228%2CCVE-2023-4863&values=15003226%2C9897254%2C764410%2C113646%2C71890%2C13212%2C431%2C669%2C44%2C9%2C3%2C3&title=Known%20CVEs%20still%20exposed%20on%20the%20internet%20%28Shodan%29&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=CVE-2018-15473%2CCVE-2023-44487%2CCVE-2022-0778%2CCVE-2017-5638%2CCVE-2014-0160%2CCVE-2019-0708%2CCVE-2020-1472%2CCVE-2021-26855%2CCVE-2022-22965%2CCVE-2023-34362%2CCVE-2021-44228%2CCVE-2023-4863&values=15003226%2C9897254%2C764410%2C113646%2C71890%2C13212%2C431%2C669%2C44%2C9%2C3%2C3&title=Known%20CVEs%20still%20exposed%20on%20the%20internet%20%28Shodan%29&unit=hosts" width="680" alt="known-cves-still-exposed"></a>

### CISA Known Exploited Vulnerabilities — [full page](docs/3-vulnerabilities/02-cisa-kev/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762"><img src="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762" width="680" alt="cisa-known-exploited-vulnerabilities"></a>

### TLS and SSH fingerprints — [full page](docs/4-fingerprints/01-tls-and-ssh/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=00000000000000000000000000000000000000000000000000000000000000%2C29d29d00029d29d00041d41d00041dcb3f8752a2d70d1dbf446fc46ab5df96%2C29d29d00029d29d21c29d29d29d29dab965d4179316d1f3a3c5dbe7d8829db%2C29d29d00029d29d00029d29d29d29da8f16e8f0a21d89b06fef5080cbd1b2a%2C29d29d00029d29d21c41d41d00041d0fc7ac8335432249e8becb757baaacec&values=9716808%2C4035455%2C2432295%2C2148551%2C1712644&title=TLS%20JARM%20fingerprint%20share%20%28port%3A443%29&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=00000000000000000000000000000000000000000000000000000000000000%2C29d29d00029d29d00041d41d00041dcb3f8752a2d70d1dbf446fc46ab5df96%2C29d29d00029d29d21c29d29d29d29dab965d4179316d1f3a3c5dbe7d8829db%2C29d29d00029d29d00029d29d29d29da8f16e8f0a21d89b06fef5080cbd1b2a%2C29d29d00029d29d21c41d41d00041d0fc7ac8335432249e8becb757baaacec&values=9716808%2C4035455%2C2432295%2C2148551%2C1712644&title=TLS%20JARM%20fingerprint%20share%20%28port%3A443%29&unit=hosts" width="680" alt="tls-and-ssh-fingerprints"></a>

### The keyless path — [full page](docs/5-method/01-keyless/en.md)

<a href="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports"><img src="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports" width="680" alt="the-keyless-path"></a>

## All pages

### Overview · 总览

- [What Shodan measures, and what this page does with it](docs/1-overview/01-about/en.md)

### Exposure · 暴露面

- [Service exposure over time](docs/2-exposure/01-trends/en.md)
- [Where the exposure is](docs/2-exposure/02-where/en.md)
- [Ports, networks, operating systems](docs/2-exposure/03-ports-networks-os/en.md)

### Vulnerabilities · 漏洞

- [Known CVEs still exposed](docs/3-vulnerabilities/01-cve-stock/en.md)
- [CISA Known Exploited Vulnerabilities](docs/3-vulnerabilities/02-cisa-kev/en.md)

### Fingerprints · 指纹

- [TLS and SSH fingerprints](docs/4-fingerprints/01-tls-and-ssh/en.md)

### Method · 方法

- [The keyless path](docs/5-method/01-keyless/en.md)

## 中文页

### 总览

- [Shodan 度量的是什么，这个页面拿它做什么](docs/1-overview/01-about/cn.md)

### 暴露面

- [服务暴露面的时间走势](docs/2-exposure/01-trends/cn.md)
- [暴露面在哪里](docs/2-exposure/02-where/cn.md)
- [端口、网络与操作系统](docs/2-exposure/03-ports-networks-os/cn.md)

### 漏洞

- [仍在暴露的已知 CVE](docs/3-vulnerabilities/01-cve-stock/cn.md)
- [CISA 已被利用漏洞目录](docs/3-vulnerabilities/02-cisa-kev/cn.md)

### 指纹

- [TLS 与 SSH 指纹](docs/4-fingerprints/01-tls-and-ssh/cn.md)

### 方法

- [免 key 的那条路](docs/5-method/01-keyless/cn.md)

## How the charts work

Every figure is a plain `https://x-cmd.com/draw?type=…` URL — rendered to a standalone SVG by [x-cmd/draw](https://github.com/x-cmd/draw). No JavaScript runs on this page; the SVG is static, printable, and AI-readable, because the parameters *are* the data.

To make your own: run `x shodan host count -f <facet> '<query>'` (or `x shodan trend search`), then hand the numbers to draw — or let your AI agent do it with the [draw skill](https://raw.githubusercontent.com/x-cmd/draw/main/SKILL.md).


## Data sources

Shodan API — [api.shodan.io](https://api.shodan.io) (`/shodan/host/count`, `vuln:` filter) · Shodan Trends — [trends.shodan.io](https://trends.shodan.io) (113 monthly buckets, 2017-06 → 2026-10) · keyless per-host endpoint — [internetdb.shodan.io](https://internetdb.shodan.io). CISA KEV — [known_exploited_vulnerabilities.csv](https://www.cisa.gov/sites/default/files/csv/known_exploited_vulnerabilities.csv), pulled by `x kev ls`. All figures queried 2026-10-29 and will drift.
