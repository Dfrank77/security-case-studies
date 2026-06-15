# Looking for a Dog Groomer, I Found a Live Infostealer Campaign

*A case study in cloaked ClickFix and the structural failure of automated abuse reporting.*

> [!NOTE]
> **Draft in progress.** This case study is actively being written and reviewed. Sections are populated incrementally as analysis is completed and screenshots are anonymized. Final version expected within the next 1-2 weeks. Substantive feedback welcome via GitHub Issues.

---

## TL;DR

*[Three to five sentences. The whole story in a paragraph. What you were doing, what you found, what you did about it, what happened, what it means.]*

---

## Background: What Is ClickFix?

*[Plain-English explanation of the attack family. Social engineering vector, technical mechanism (clipboard + user-executed PowerShell), typical payload (StealC, Lumma, Vidar), growth through 2025-2026. Cite CISA, Microsoft Threat Intelligence, Proofpoint TA571, Malwarebytes.]*

---

## The Encounter

*[Human-narrative section. What you were doing, what you saw, the specific details that made you stop. The brief flash of real site before the overlay dropped is worth emphasizing.]*

![Anonymized fake CAPTCHA overlay](./evidence/clickfix-overlay-anonymized.png)

---

## Initial Verification

*[VirusTotal clean, urlscan clean, 117 transactions across 22 domains in 4 countries. Why scanners hit the clean version.]*

![urlscan summary, URL redacted](./evidence/urlscan-summary-redacted.png)

---

## The Cloaking Mechanism

*[Technical analysis. Why scanners get the clean version (no Google referrer, datacenter IPs, headless browsers). Why real visitors get the attack (matching referrer, residential IP, fresh session, US geo). Deferred-injection pattern: setTimeout / DOMContentLoaded, then high-z-index overlay. The visible flash of real content before the overlay is direct observation worth highlighting.]*

---

## Disclosure: What I Did

*[Chronology in order. Private email to site owner. Google Safe Browsing report. Cloudflare abuse report with brand impersonation flag. Note what was NOT done: no unauthorized testing, no public URL disclosure.]*

| Day | Action | Channel |
|-----|--------|---------|
| Day 0 | Discovered compromise during normal browsing | — |
| Day 0 | Sent private disclosure email to site owner | Email |
| Day 1 | Submitted Google Safe Browsing report | Web form |
| Day 1 | Submitted Cloudflare abuse report (with brand impersonation flag) | Web form |
| Day 3 | Confirmed site still compromised; no owner response; both platform reports auto-closed | — |

---

## The Outcomes

*[Owner: no response. Google Safe Browsing: auto-closed "no unsafe content found." Cloudflare: same outcome. Net protective action delivered: zero. Include redacted screenshots of rejection responses.]*

![Google Safe Browsing rejection response](./evidence/google-safebrowsing-rejection-redacted.png)
![Cloudflare abuse rejection response](./evidence/cloudflare-abuse-rejection-redacted.png)

---

## The Structural Problem

*[The thesis. Automated abuse reporting at every major platform depends on the platform reproducing the malicious behavior. Cloaked attacks are designed specifically to defeat reproduction. The more sophisticated the cloaking, the less likely automated review confirms the attack, regardless of how strong the reporter's evidence is. Result: real attacks against real victims systematically under-actioned because the protection layer requires evidence the attack is designed not to give it.]*

---

## What This Means

**For end users:** never paste anything into an elevated terminal because a website told you to. Ever. Under any framing.

**For SMB site owners:** automated scanners reporting clean is not evidence of safety. WordPress sites with old plugins are mass-compromise targets. Monitor for unexpected JS injections.

**For platforms operating abuse pipelines:** the cloaking-defeats-automation gap needs human-in-the-loop escalation paths that accept reporter-submitted evidence (screen recordings, HAR files, reproduction context) without requiring the platform's own crawlers to confirm.

**For aspiring defenders:** sometimes the most valuable threat work is documenting what doesn't work, not what does.

---

## Indicators of Compromise

Generic IOCs for the ClickFix family:

- Fake Cloudflare-branded verification overlay with PowerShell instructions
- Win+X / Win+R keystroke instructions in the overlay
- Clipboard-injected commands containing `powershell`, `iex`, `Invoke-Expression`, `DownloadString`, or base64-encoded payloads
- Deferred script execution after `DOMContentLoaded` or via `setTimeout`
- High-z-index full-screen overlay element appended to document body
- Brief render of legitimate page content before the overlay appears

---

## Acknowledgments and Disclosure Statement

The site owner has not been publicly identified to avoid punitive disclosure. Standard responsible disclosure practice was followed: private notification first, public discussion of the pattern only (not the specific site), reasonable window for remediation before publishing.

The author has no affiliation with the affected site, the hosting provider, or any vendor mentioned. This is observational research conducted as a private citizen, not under any commercial engagement.

---

## References

- [CISA: ClickFix social engineering technique](https://www.cisa.gov/) *(replace with specific advisory URL)*
- [Microsoft Threat Intelligence: Lumma Stealer ClickFix campaigns](https://www.microsoft.com/en-us/security/blog/) *(replace with specific post)*
- [Proofpoint: TA571 ClearFake ClickFix campaigns](https://www.proofpoint.com/us/threat-insight) *(replace with specific post)*
- [Malwarebytes: WordPress mass compromise campaigns](https://www.malwarebytes.com/blog) *(replace with specific post)*

---

*Last updated: 2026-06-15. Written by Darius Frank. Discussion welcome via [GitHub Issues](../../issues).*
