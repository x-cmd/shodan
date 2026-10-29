# CISA Known Exploited Vulnerabilities

## What the KEV list actually is

CISA's Known Exploited Vulnerabilities catalog is not a vulnerability database and not a severity list. It is a patching order. An entry means we have evidence this is being exploited in the wild and US federal agencies must patch it by this date.

Being on the list is a statement about exploitation, not about how bad the bug is. Some entries are CVSS 4.0; some are technical debt that only became exploitable because everything else got patched. The catalog has 1,762 entries.

## By vendor

```text
https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs"><img src="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs" width="780" alt="cisa-known-exploited-vulnera"></a>

## By vendor, split by ransomware

```text
https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs
```

<a href="https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs"><img src="https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs" width="780" alt="cisa-known-exploited-vulnera"></a>

361 of the 1,762 entries carry `knownRansomwareCampaignUse = Known`.

## Reading the first chart

Microsoft dominates with 390 entries, and that number flatters it. Microsoft ships dozens of separately-versioned components, each with its own patch cycle, and each one generates its own CVE. A large vendor with a large product surface will always top a per-vendor count. Cisco (101), Apple (95) and Adobe (83) follow, all three of them appliances or endpoint software where the customer does not control the update path.

## Reading the second chart

The more interesting cut is not who has the most KEVs but who is being actively monetised.

Fortinet has 14 entries and more than half of them are ransomware-linked. SonicWall has 13 with a high ransomware share. Ivanti has 12, and the majority are ransomware-linked. Oracle has 13.

These are VPN concentrators, firewalls and edge appliances, which is precisely the class of device ransomware operators get in through: internet-facing, credential-bearing, frequently unpatched because the update path runs through a support contract.

So the pairing is the point. KEV membership tells you whether a product is being exploited. The ransomware column tells you how that exploitation is monetised. For edge appliances the two collapse into the same thing, and for consumer software they are completely different.

A product in the first column but not the second is being exploited for access, espionage or botnets. A product in both is being exploited for money.

## Why this is worth a page of its own

The KEV catalog is public, free, updated daily, and, for most people, completely unreadable. It is a CSV with 12 columns. Rendering it as a picture is the entire point: a regulatory order list that nobody reads becomes a chart that a network team can act on in a meeting.

The same chart works for any organisation's own vulnerability management, if you map your patch backlog onto the same two columns.

## Caveats

The KEV catalog is US federal scope. Its contents reflect what US agencies are told about, and it is not a global census of exploitation. "Ransomware-linked" means CISA has attribution for that campaign, which is not the same as this being the only way it is exploited. And vendor counts reflect product surface area rather than security posture.

> Reproduce: `x kev ls`, which pulls CISA's `known_exploited_vulnerabilities.csv` directly. The catalogue has 12 columns including `knownRansomwareCampaignUse`, `dueDate`, `requiredAction` and `cwes`.
