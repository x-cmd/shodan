---
name: shodan
description: Charts and data for what the internet exposes — service exposure over time, geography, ports, networks, OS, known CVEs still reachable, CISA KEV, and TLS / SSH fingerprints. Use when the user asks about "exposed hosts", "internet exposure", "what is running on my IP", "Shodan facets", "CVE still exposed", "CISA KEV", "TLS fingerprint", "JA3", "JARM", or wants an attack-surface picture of their own assets.
metadata: type=reference, source=shodan-api+internetdb+cisa-kev, schema=yaml-2-doc, refresh=manual, license=apache-2.0, scope=exposure-vulnerabilities-fingerprints
---

# x-cmd/shodan — the exposed internet, rendered

Two consumption paths. Pick whichever fits.

## 1. Keyless: one curl per host

No account, no key, no rate-limit negotiation:

```sh
curl -s https://internetdb.shodan.io/1.1.1.1
```

```json
{"cpes":["cpe:/a:cloudflare:cloudflare"],
 "hostnames":["one.one.one.one"],
 "ip":"1.1.1.1",
 "ports":[53,69,80,443,2053,2082,2087,2096,8080,8443,8880],
 "tags":["pqc"],
 "vulns":[]}
```

Aggregate any list of addresses with jq:

```sh
for ip in $(cat ips.txt); do curl -s "https://internetdb.shodan.io/$ip"; done \
  | jq -r '.ports[]?' | sort | uniq -c | sort -rn
```

## 2. With a key: aggregation, trends, and the full facet list

```bash
x shodan --cfg key=<your apikey>       # set the key once
x shodan host count -f country 'product:nginx'
x shodan trend search product:nginx
x shodan trend facets                   # the 39 dimensions
x shodan cve count --product nginx --is_kev true
x shodan geo ping 8.8.8.8
x kev ls                                # CISA KEV, no key needed
```

**Argument order matters today**: `-f <facet>` must come *before* the query.
`x shodan host count 'product:nginx' -f country` silently swallows the flag
(see x-cmd/x-cmd#515).

## 3. Read the recorded breakdowns

```sh
curl -fsSL https://raw.githubusercontent.com/x-cmd/shodan/main/data/facets-2026-10-29.yaml
curl -fsSL https://raw.githubusercontent.com/x-cmd/shodan/main/data/cisa-kev-2026-10-29.yaml
```

## The 39 facets

```
asn  city  country  cpe  domain  has_screenshot  hash
http.component  http.component_category  http.favicon.hash  http.html_hash
http.robots_hash  http.status  http.title  http.waf
ip  isp  mongodb.database.name  org  os  port  product
redis.key  region  rsync.module  screenshot.label
ssh.fingerprint  ssl.alpn  ssl.cert.fingerprint  ssl.cert.issuer.cn
ssl.cert.serial  ssl.cert.subject.cn  ssl.ja3s  ssl.jarm  ssl.version
state  tag  version  vuln
```

## Turning it into a chart

```
https://x-cmd.com/draw?type=hbar&labels=US,CN&values=15196466,9905515&title=Exposed%20nginx%20by%20country
```

## Watch out

- **Counts are Shodan-observable, not totals.** Shodan counts what its own fleet
  scanned, and its coverage changes over time — which pushes trends upward for
  reasons unrelated to the software. Read trends comparatively.
- **Use `vuln:`, not `vulns:`.** The latter returns zero.
- **A "Not Found" from internetdb means "we have nothing on this host", not
  "the host is down".**
- **Exposure count measures how expensive a fix is**, not how dangerous a bug is.
