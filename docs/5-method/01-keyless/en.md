# The keyless path

## Everything above needed a key. This does not.

Shodan's main API requires an account and an API key. But there is a second endpoint, `internetdb.shodan.io`, that returns the same shape of data for a single host with **no key, no account, and no rate-limit negotiation**:

```sh
curl -s https://internetdb.shodan.io/1.1.1.1
```

```json
{"cpes":["cpe:/a:cloudflare:cloudflare"],
 "hostnames":["one.one.one.one","wtby.wlc.theeyecaregroup.com"],
 "ip":"1.1.1.1",
 "ports":[53,69,80,443,2053,2082,2087,2096,8080,8443,8880],
 "tags":["pqc"],
 "vulns":[]}
```

One `curl` per IP. That is the whole interface.

## Open ports per host, across ten well-known hosts

```text
https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports
```

<a href="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports"><img src="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports" width="780" alt="the-keyless-path"></a>

## Port frequency across the same ten hosts

```text
https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts
```

<a href="https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts"><img src="https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts" width="780" alt="the-keyless-path"></a>

## What the sample shows

Two hosts — Cloudflare's `104.16.0.1` with 13 ports and `162.159.1.1` with 12 — carry the full set of **alternative HTTP ports** (2052, 2082, 2083, 2086, 2087, 2095, 2096, 8080, 8443, 8880). These are Cloudflare's spectrum/proxy ports, and they are exactly the ports that a "which ports should I open?" conversation never mentions.

At the other end, `8.8.8.8` and `13.107.42.14` expose two ports each. The spread from 2 to 13 ports is not a fact about the organisations — it is a fact about the **products** they run and the **degree to which those products have been customised**.

## When to use which

**Use internetdb when:**
- You have been handed a list of IPs — a leaked credential list, a phishing target list, an incident-response list — and want a quick triage. This is the fastest possible first pass, because it needs no setup at all.
- You are writing a script and want a dependency-free check.
- You want a per-host answer and do not need aggregation.

**Use the authenticated API when:**
- You need **aggregation** — the whole point of this site. Facet breakdowns need `/shodan/host/count` with a query and a facets parameter.
- You need the trends history (113 months) or the scan/stream endpoints.
- You need to be sure of the counts, rather than sampling.

## A script to aggregate it yourself

Because the endpoint is so simple, the whole "exposure picture of a list of IPs" is about fifteen lines:

```sh
for ip in $(cat ips.txt); do
  curl -s "https://internetdb.shodan.io/$ip"
done | jq -r '.ports[]?' | sort | uniq -c | sort -rn
```

Add `jq` grouping and you have a port histogram for any set of addresses — a new vendor's range, a customer's estate, a leaked list — without an account, in seconds.

## Caveats

- internetdb covers a **subset** of Shodan's index — it is optimised for the most commonly queried fields, and does not expose facets, trends or scanning.
- A host that does not appear in Shodan's index returns `{"detail":"Not Found"}`, which means "we have nothing", **not** "the host is down". Those are very different claims.
- The data is a snapshot of when Shodan last scanned that host, which may be months old.

> Reproduce: loop over any IP list with `curl -s https://internetdb.shodan.io/<ip>` and aggregate the `ports` array.
