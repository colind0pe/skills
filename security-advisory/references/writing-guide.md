# Security Advisory Writing Guide

Detailed guidance for Phase 3 and Phase 4 of the advisory workflow: drafting the email structure, applying the writing style, and performing the final self-review.

---

## Email Subject Line Format

```
[INT] Security Advisory – [SHORT TITLE] ([CVE-ID if applicable])
```

- Use `–` (en-dash) as separator, not `-`
- `[INT]` prefix is standard for all internal-facing advisories
- For security events without a CVE, use a descriptive title: e.g. "Exposed TheGentlemen Ransomware Toolkit"
- For multiple CVEs: "(CVE-2025-55182 / CVE-2025-66478)"

---

## Section-by-Section Writing Guidance

The template in `assets/advisory-template.md` defines the exact section order and layout. This guide explains how to write each section well.

### Background
- Use a narrative paragraph, not bullet points — this section tells the story of what happened
- Lead with the affected product/component, then the root cause
- Be specific about the flaw type (e.g., "improper input sanitization" not "security vulnerability")
- Include disclosure context (when, by whom) if it adds urgency

### Impact
- CVSS score is the anchor — always include it with the exact number and rating
- Attack vector and exploitability should help the client quickly assess exposure
- Consequences should reflect realistic worst-case scenarios, not theoretical ones

### Attack Observation
- This is the most time-sensitive section — clients need to know if they're under active threat
- If no exploitation is confirmed, say so explicitly: "As of [date], no confirmed exploitation has been reported."
- Include MITRE ATT&CK technique IDs when describing TTPs (e.g., `credential dumping (T1003.001)`)
- If a PoC exists, note whether it's public or private

### Affected Versions
- Use exact version numbers (e.g., `4.0 through 4.21.8`), not vague ranges
- For cloud/managed services, describe the exposure condition instead
- Use a table when multiple products are affected

### IOCs
- Include only when IOCs are actually available — omit this section entirely otherwise
- Always defang IPs with `[.]` notation (e.g., `104.28.244[.]115`)
- Format each IOC as: `**[Type]**: [defanged indicator] - [description]`

### Remediation
- Every action item must be tagged with `[Client]` or `[EnSOC]` to assign responsibility:
  - `[Client]` = action the client must take
  - `[EnSOC]` = action Ensign SOC will perform / monitor for
- Provide exact patch version numbers and vendor advisory links
- Distinguish between patches (primary fix) and workarounds (temporary mitigations)

### Recommendations
Additional strategic or operational guidance beyond the immediate remediation:
- Hardening measures
- Monitoring rules
- User awareness

Also tagged with `[Client]` or `[EnSOC]`

> Note: Remediation and Recommendations may be merged into a single "Remediation & Recommendation" section when the advisory is straightforward.

---

## Writing Style

### Tone
- Professional but not overly stiff — readable and urgent when warranted
- Direct and factual; avoid marketing language or excessive hedging
- Balanced: communicate risk clearly without causing panic

### Language Patterns
- Use active voice: "Attackers can execute..." not "Code execution may be possible..."
- State exploitation status clearly: "No confirmed exploitation in the wild" vs "Actively exploited since [date]"
- Use precise language: "unauthenticated remote attacker" not "hackers"
- Numbers and scores should be exact: CVSS 9.1, not "very high"

---

## Advisory Type Quick Reference

| Scenario                       | Subject Prefix                          | Key Sections to Emphasize                    | IOC Priority           |
| ------------------------------ | --------------------------------------- | -------------------------------------------- | ---------------------- |
| CVE with patch available       | `CVE-XXXX-XXXXX – [Product]`            | Affected Versions, Remediation               | Low (unless exploited) |
| CVE actively exploited (0-day) | `CVE-XXXX: Actively Exploited Zero-Day` | Attack Observation, IOCs, urgent Remediation | High                   |
| Ransomware campaign            | `[Threat Group] Ransomware Activity`    | TTPs, IOCs, Affected Environments            | Critical               |
| Threat toolkit exposed         | `Exposed [Name] Toolkit`                | IOCs, Attack Observation, Recommendations    | Critical               |
| AI/emerging threat             | `[Risk Type] in [Product/Technology]`   | Background, Impact, Recommendations          | Medium                 |

---

## Quality Checklist (Self-review before finalizing)

Before presenting the advisory, verify it answers these questions for the client:

- [ ] **What happened?** — Is the Background section clear and factual?  
- [ ] **How bad is it?** — Is the severity/CVSS communicated? Are the worst-case consequences stated?
- [ ] **Is it being exploited?** — Is exploitation status explicitly stated (yes/no/PoC available)?
- [ ] **Am I affected?** — Are the affected versions/configurations listed clearly?
- [ ] **What do I need to do?** — Are client actions clearly tagged with `[Client]`?
- [ ] **What will Ensign do?** — Are Ensign SOC monitoring actions tagged with `[EnSOC]`?
- [ ] **Are IOCs included?** — If exploitation is observed, are network/file indicators provided?
- [ ] **Are sources cited?** — Are vendor advisories, CVE links, or research sources referenced with real URLs?
- [ ] **Is the tone appropriate?** — Is urgency proportional to actual severity?
- [ ] **Is it complete?** — No placeholder text like "[TBD]" or empty sections?
