# Contributing

## Repository layout

```
shodan/
├── README.md          English landing page — the charts, indexed
├── README.cn.md       中文 landing page
├── SKILL.md           How an AI agent or a script consumes this repo
├── CONTRIBUTING.md    this file
├── LICENSE            Apache-2.0 for the repo's own content
├── data/              the underlying data, as plain YAML
└── docs/              one directory per page; the source of shodan.com/<name>/*
```

## Pages

Each page lives in `docs/<chapter>/<NN>-<slug>/` and holds:

| File | Purpose |
|---|---|
| `en.md` | English page |
| `cn.md` | 中文页 |
| `faq.yml` | one question/answer pair, consumed by the site's FAQ block |

- `docs/1-overview/01-about/` — What Shodan measures, and what this page does with it / Shodan 度量的是什么，这个页面拿它做什么
- `docs/2-exposure/01-trends/` — Service exposure over time / 服务暴露面的时间走势
- `docs/2-exposure/02-where/` — Where the exposure is / 暴露面在哪里
- `docs/2-exposure/03-ports-networks-os/` — Ports, networks, operating systems / 端口、网络与操作系统
- `docs/3-vulnerabilities/01-cve-stock/` — Known CVEs still exposed / 仍在暴露的已知 CVE
- `docs/3-vulnerabilities/02-cisa-kev/` — CISA Known Exploited Vulnerabilities / CISA 已被利用漏洞目录
- `docs/4-fingerprints/01-tls-and-ssh/` — TLS and SSH fingerprints / TLS 与 SSH 指纹
- `docs/5-method/01-keyless/` — The keyless path / 免 key 的那条路

## The naming rule that the site depends on

The site builds `x-cmd.com/<name>/<slug>` from the **leaf directory name**, with
the numeric prefix stripped. So:

| Directory | Route |
|---|---|
| `docs/1-overview/01-about/` | the site index |
| `docs/2-registry/01-rir-allocation/` | `x-cmd.com/shodan/rir-allocation` |

Two rules follow, and breaking either breaks the build:

1. **Leaf names must be unique across the whole repository.** They become
   top-level routes, so `02-x/` in one chapter and `03-x/` in another would
   collide.
2. **The first page of the first chapter is the index.** It is `01-*`, not
   `02-*`.

## The charts

Every figure is a plain `https://x-cmd.com/draw?type=…` URL, rendered to a
standalone SVG by [x-cmd/draw](https://github.com/x-cmd/draw). The URL is written
out in full in a ```text``` block, followed by an `<a href><img></a>` pair so
the page shows the picture and keeps the parameters visible and copyable.

**Never replace a draw URL with an image file.** The URL *is* the data: it is
machine-readable, editable, and the site re-renders it server-side.

## Shodan's API needs a key (set it with `x shodan --cfg key=<apikey>`); the CISA KEV catalog is public and needs none. Both change daily, so the `as_of` date on each file is load-bearing — a chart without one is a claim without a timestamp.

## Before you open a pull request

- [ ] `npx rumdl check .` passes (or the new finding is suppressed with a
      reason in `.rumdl.toml`)
- [ ] every new draw URL returns 200 — check it with
      `curl -s -o /dev/null -w '%{http_code}' '<url>'`
- [ ] the page exists in **both** `en.md` and `cn.md`
- [ ] `faq.yml` has an entry for the new page
- [ ] the numbers are re-derived from the primary source, not copied from the
      previous page
