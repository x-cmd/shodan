# What Shodan measures, and what this page does with it

## The difference between Shodan and a search engine

A web search engine asks: *what pages say?* Shodan asks a different question: *what is running?* It connects to hosts and looks at what they expose — open ports, the banner each service sends, the TLS certificate, the HTTP headers, the operating system fingerprint. There is no index of pages; there is an index of **hosts and the services they run**.

That is why Shodan's results are organised around **facets** rather than keywords. A facet is a dimension you can break a result set down by. Shodan exposes 39 of them:

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

Read that list as a schema for the internet: where things are (`country`, `city`, `asn`, `org`, `isp`), what they run (`port`, `product`, `version`, `cpe`), what they are built on (`os`), what they serve (`http.title`, `http.status`, `http.waf`), what crypto they use (`ssl.jarm`, `ssl.ja3s`, `ssl.cert.*`, `ssl.version`), what keys they reuse (`ssh.fingerprint`), and what is wrong with them (`vuln`).

## The gap this page fills

Here is the thing about Shodan's output that is easy to miss: **a facet breakdown arrives as JSON.** And JSON, dropped into a GitHub issue, a chat message, or a blog post, is something people scroll past. A number in a table is better; a picture is better still, because the *shape* of a distribution is often the finding, and JSON hides shape.

So this page does one thing: **it takes the facet breakdown and renders it.** Not a screenshot — a chart whose data is still queryable, because a chart here is just a URL with the parameters written out.

And that last property is the point. Because the URL is editable, **the query is editable**. Change `product:nginx` to `org:yourcompany` or `net:10.0.0.0/8` and the same template becomes a picture of *your* attack surface, not of nginx's. Shodan's own users can do this themselves, with the facet table at the bottom of this page as the menu.

## How to read the rest of this site

- **Exposure** — how much of the internet is running a given service, how that has changed, and where it is concentrated.
- **Vulnerabilities** — which known flaws are still reachable, and which of those are being exploited on purpose.
- **Fingerprints** — what the TLS and SSH layers reveal that a certificate does not.

## Scope and honesty

Every figure here was queried on **2026-10-29** against Shodan's API and will drift. Shodan counts what *it* has scanned, so its coverage is a property of Shodan's scanning fleet and its vantage points — not a census of the internet. Counts are best read as orders of magnitude and as ratios, not as precise totals.

Where a number has a specific bias (IPv4 allocation counts are *allocations*, not addresses; country breakdowns reflect where hosting is, not where software is used), the page says so.

> CLI: `x shodan` — see [x-cmd.com/mod/shodan](https://x-cmd.com/mod/shodan)
> Data: [api.shodan.io](https://api.shodan.io) · [internetdb.shodan.io](https://internetdb.shodan.io) (keyless)
> The facet list is live: `x shodan trend facets`
