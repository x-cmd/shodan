# TLS and SSH fingerprints

## The problem with certificates

A TLS certificate tells you who is presenting. It does not tell you what is terminating the connection. Those are different questions, and in the modern internet they frequently have different answers.

A host serving a site through a CDN presents the CDN's certificate, a perfectly valid certificate for the domain you asked for. Behind it may be nginx, or Apache, or a Go application, or a Python Flask dev server, or a four-year-old appliance with a default TLS stack. The certificate is identical in all of those cases.

Finding out what is really there means fingerprinting the handshake itself.

## JARM: fingerprinting the TLS stack

```text
https://x-cmd.com/draw?type=hbar&labels=00000000000000000000000000000000000000000000000000000000000000%2C29d29d00029d29d00041d41d00041dcb3f8752a2d70d1dbf446fc46ab5df96%2C29d29d00029d29d21c29d29d29d29dab965d4179316d1f3a3c5dbe7d8829db%2C29d29d00029d29d00029d29d29d29da8f16e8f0a21d89b06fef5080cbd1b2a%2C29d29d00029d29d21c41d41d00041d0fc7ac8335432249e8becb757baaacec&values=9716808%2C4035455%2C2432295%2C2148551%2C1712644&title=TLS%20JARM%20fingerprint%20share%20%28port%3A443%29&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=00000000000000000000000000000000000000000000000000000000000000%2C29d29d00029d29d00041d41d00041dcb3f8752a2d70d1dbf446fc46ab5df96%2C29d29d00029d29d21c29d29d29d29dab965d4179316d1f3a3c5dbe7d8829db%2C29d29d00029d29d00029d29d29d29da8f16e8f0a21d89b06fef5080cbd1b2a%2C29d29d00029d29d21c41d41d00041d0fc7ac8335432249e8becb757baaacec&values=9716808%2C4035455%2C2432295%2C2148551%2C1712644&title=TLS%20JARM%20fingerprint%20share%20%28port%3A443%29&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=00000000000000000000000000000000000000000000000000000000000000%2C29d29d00029d29d00041d41d00041dcb3f8752a2d70d1dbf446fc46ab5df96%2C29d29d00029d29d21c29d29d29d29dab965d4179316d1f3a3c5dbe7d8829db%2C29d29d00029d29d00029d29d29d29da8f16e8f0a21d89b06fef5080cbd1b2a%2C29d29d00029d29d21c41d41d00041d0fc7ac8335432249e8becb757baaacec&values=9716808%2C4035455%2C2432295%2C2148551%2C1712644&title=TLS%20JARM%20fingerprint%20share%20%28port%3A443%29&unit=hosts" width="780" alt="tls-and-ssh-fingerprints"></a>

JARM is an active fingerprint. It works by sending a deliberately malformed TLS ClientHello and observing how the server reacts. The response pattern, which cipher suites are offered and in what order, which extensions appear, how errors are formatted, is a function of the server's TLS implementation. Two servers running different stacks produce different JARM values even when they serve identical content over identical certificates.

The distribution shows how concentrated that is: a handful of fingerprints cover millions of hosts. Each one is a distinct TLS implementation deployed at scale, and each has its own quirks, bugs and mitigations.

Practically, this lets you count how many real implementations sit behind one apparent service, work out which advisories apply to each, and flag any host whose fingerprint does not match its claimed stack.

There is a related facet, `ssl.ja3s`, which fingerprints the client side instead, useful for identifying what software is connecting to you.

## SSH key fingerprints: how many people share one key

```text
https://x-cmd.com/draw?type=hbar&labels=2f%3A1c%3A34%3Ac9%3A4c%3A56%3A12%3A6c%3Ace%3Af2%3A10%3Aee%3A0f%3A3e%3A41%3Afe%2Cd8%3A0e%3A4d%3Ad3%3Ad7%3A00%3Ad7%3Aaa%3A8f%3A69%3A9f%3Aec%3Aaf%3A90%3Ab3%3A16%2C78%3Ae3%3Ac6%3Ad0%3A2d%3A4c%3Af8%3A3e%3A87%3A8c%3Abf%3A34%3Ad6%3A72%3A6f%3A46%2C8b%3A75%3A88%3A08%3A41%3A78%3A11%3A5b%3A49%3A68%3A11%3A42%3A64%3A12%3A6d%3A49%2C3e%3A75%3A5f%3A41%3A69%3Ad1%3A67%3A42%3A5e%3Ad1%3A03%3Adb%3Ac8%3A04%3A90%3A84&values=142337%2C39179%2C34007%2C27799%2C21855&title=SSH%20keys%20reused%20across%20the%20internet%20%28port%3A22%29&unit=hosts
```

<a href="https://x-cmd.com/draw?type=hbar&labels=2f%3A1c%3A34%3Ac9%3A4c%3A56%3A12%3A6c%3Ace%3Af2%3A10%3Aee%3A0f%3A3e%3A41%3Afe%2Cd8%3A0e%3A4d%3Ad3%3Ad7%3A00%3Ad7%3Aaa%3A8f%3A69%3A9f%3Aec%3Aaf%3A90%3Ab3%3A16%2C78%3Ae3%3Ac6%3Ad0%3A2d%3A4c%3Af8%3A3e%3A87%3A8c%3Abf%3A34%3Ad6%3A72%3A6f%3A46%2C8b%3A75%3A88%3A08%3A41%3A78%3A11%3A5b%3A49%3A68%3A11%3A42%3A64%3A12%3A6d%3A49%2C3e%3A75%3A5f%3A41%3A69%3Ad1%3A67%3A42%3A5e%3Ad1%3A03%3Adb%3Ac8%3A04%3A90%3A84&values=142337%2C39179%2C34007%2C27799%2C21855&title=SSH%20keys%20reused%20across%20the%20internet%20%28port%3A22%29&unit=hosts"><img src="https://x-cmd.com/draw?type=hbar&labels=2f%3A1c%3A34%3Ac9%3A4c%3A56%3A12%3A6c%3Ace%3Af2%3A10%3Aee%3A0f%3A3e%3A41%3Afe%2Cd8%3A0e%3A4d%3Ad3%3Ad7%3A00%3Ad7%3Aaa%3A8f%3A69%3A9f%3Aec%3Aaf%3A90%3Ab3%3A16%2C78%3Ae3%3Ac6%3Ad0%3A2d%3A4c%3Af8%3A3e%3A87%3A8c%3Abf%3A34%3Ad6%3A72%3A6f%3A46%2C8b%3A75%3A88%3A08%3A41%3A78%3A11%3A5b%3A49%3A68%3A11%3A42%3A64%3A12%3A6d%3A49%2C3e%3A75%3A5f%3A41%3A69%3Ad1%3A67%3A42%3A5e%3Ad1%3A03%3Adb%3Ac8%3A04%3A90%3A84&values=142337%2C39179%2C34007%2C27799%2C21855&title=SSH%20keys%20reused%20across%20the%20internet%20%28port%3A22%29&unit=hosts" width="780" alt="tls-and-ssh-fingerprints"></a>

The `ssh.fingerprint` facet counts hosts by the public key their SSH server offers. A small number of fingerprints dominating the distribution means a large number of hosts share the same private key.

That matters for two reasons. It is a security problem, because one compromise, a leaked backup or a decommissioned machine, opens thousands of hosts at once, and those hosts become indistinguishable to an attacker and to a defender. It is also a fingerprinting tool, because a host offering a widely-shared key is trivially identifiable and the population can be counted and tracked.

## Why both cuts matter for asset inventory

Together they turn a list of hosts into a list of implementations. The TLS fingerprint says what stack is terminating the connection, and the SSH fingerprint says what key the host uses and therefore which population it belongs to.

Two hosts can look identical from the outside, same port, same banner, same certificate, and still differ in both of these respects. Most asset inventories stop at the certificate. These are the next two questions.

## Caveats

Fingerprints identify the implementation, not the version, so a patched and an unpatched build of the same stack share a fingerprint. JARM values depend on the library version and its configuration, and some stacks let configuration change the fingerprint. And fingerprint distributions are shaped by which stacks are popular rather than by which are secure.

> Reproduce: `x shodan host count -f ssl.jarm 'port:443'` and `x shodan host count -f ssh.fingerprint 'port:22'`.
> Remember that `-f` comes before the query, see x-cmd/x-cmd#515.
