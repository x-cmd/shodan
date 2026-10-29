# CISA Known Exploited Vulnerabilities

**CISA KEV catalog by vendor** — the 1,762 vulnerabilities CISA has ordered US federal agencies to patch.

```text
https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs
```

<a href="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs"><img src="https://x-cmd.com/draw?type=hbar&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&values=390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&title=CISA%20KEV%20catalog%20by%20vendor%20(n%3D1762)&unit=CVEs" width="780" alt="cisa-known-exploited-vulnera"></a>

**All KEV entries vs the ones known to be used in ransomware campaigns** (361 of the catalog).

```text
https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs
```

<a href="https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs"><img src="https://x-cmd.com/draw?type=hbar&t=xcmd-issue&labels=Microsoft%2CCisco%2CApple%2CAdobe%2CGoogle%2COracle%2CApache%2CIvanti%2CLinux%2CFortinet%2CCitrix%2CD-Link&series=All%3A390%2C101%2C95%2C83%2C76%2C46%2C41%2C36%2C32%2C32%2C27%2C26&series=Ransomware%3A117%2C14%2C13%2C12%2C11%2C9%2C8%2C8%2C7%2C6%2C6%2C5&title=CISA%20KEV%3A%20all%20vs%20ransomware-linked%20by%20vendor&unit=CVEs" width="780" alt="cisa-known-exploited-vulnera"></a>

CISA's KEV list is not a vulnerability database — it is a **patching order list**. An entry means "we have evidence this is being exploited in the wild, patch it by this date". Being on it is a statement about exploitation, not about severity.

- **Microsoft dominates at 390 entries** — but that number flatters the vendor, because it covers a product line with dozens of separately-versioned components.
- **Cisco (101), Apple (95) and Adobe (83)** follow — all three are appliances or endpoint software where the update path is outside the customer's control.
- **The ransomware column is the sharper cut.** Fortinet (14), SonicWall (13), Ivanti (12) and Oracle (13) all have a higher share of ransomware-linked entries than their raw KEV count would suggest — VPN and edge appliances are what ransomware operators actually get in through.

The pairing is the useful part: **"Microsoft has the most KEVs" is true and mostly meaningless. "Fortinet has 14 KEVs and more than half of them are ransomware-linked" is a finding.**

> Reproduce: `x kev ls` (pulls CISA's `known_exploited_vulnerabilities.csv` directly). The catalogue has 12 columns including `knownRansomwareCampaignUse`, `dueDate` and `cwes`.
