# About this page

Shodan indexes every host it can reach on the internet and answers questions about them. It already has **39 query facets** — the dimensions you can break a search down by:

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

What Shodan did **not** have was a way to hand those answers to someone as a picture. A facet breakdown is JSON; a JSON blob in a GitHub issue, a blog post, or a chat message is something people scroll past.

That is what this page is: **the facet breakdown, rendered**. And because a chart URL is just parameters, the query is replaceable — swap `product:nginx` for `org:yourcompany` or `net:10.0.0.0/8` and the same template becomes a picture of *your* attack surface.

> CLI: `x shodan` — see [x-cmd.com/mod/shodan](https://x-cmd.com/mod/shodan)
> Data: [api.shodan.io](https://api.shodan.io) · [internetdb.shodan.io](https://internetdb.shodan.io) (keyless)
> All figures below were queried on **2026-10-29** and will drift.
