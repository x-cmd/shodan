# The keyless path

Everything above used the authenticated API. There is a second, **keyless** endpoint that returns the same shape for a single host:

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

**Open ports per host, across ten well-known hosts.**

```text
https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports
```

<a href="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports"><img src="https://x-cmd.com/draw?type=hbar&labels=104.16.0.1%2C162.159.1.1%2C1.1.1.1%2C208.67.222.222%2C9.9.9.9%2C140.82.121.4%2C8.8.8.8%2C151.101.1.140%2C13.107.42.14%2C185.199.108.153&values=13%2C12%2C11%2C4%2C3%2C3%2C2%2C2%2C2%2C2&title=Shodan%20InternetDB%3A%20open%20ports%20per%20host&unit=ports" width="780" alt="the-keyless-path"></a>

**Port frequency across the same ten hosts.**

```text
https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts
```

<a href="https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts"><img src="https://x-cmd.com/draw?type=bar&labels=443%2C80%2C53%2C2082%2C2087%2C2096%2C8080%2C8443%2C2052%2C2053%2C2083%2C2086%2C2095%2C8880%2C22%2C5353%2C69%2C853&values=10%2C8%2C5%2C3%2C3%2C3%2C3%2C3%2C2%2C2%2C2%2C2%2C2%2C2%2C1%2C1%2C1%2C1&title=Shodan%20InternetDB%3A%20port%20frequency%20across%2010%20hosts&unit=hosts" width="780" alt="the-keyless-path"></a>

No API key, no account, no rate-limit negotiation — one `curl` per IP. That makes it the right tool for a **first look at a list of IPs you just got handed**: a leaked credential list, a phishing target list, an incident-response list of addresses to check.

The shape of the ten-host sample is a fair summary of the modern edge: Cloudflare's `104.16.0.1` (13 ports) and `162.159.1.1` (12) carry the full alternative-HTTP-port set, while `8.8.8.8` and `13.107.42.14` expose two ports each.

> Reproduce: loop over any IP list with `curl -s https://internetdb.shodan.io/<ip>` and aggregate the `ports` array.
