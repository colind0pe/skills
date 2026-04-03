# Security Advisory Template & Style Guide

Extracted and distilled from 13 real advisory emails sent to clients.

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

## Email Body Structure

### Opening Salutation
```
Dear [Client / Team / client],
```
Use "Dear Client," or "Dear Team," — never use the internal team note in the client-facing body.

### One-Liner Hook (Optional but recommended)
A single sentence that concisely states what happened and why the client should care. This sets the tone.

**Example:**
> "A critical vulnerability has been disclosed in Samba, tracked as CVE-2025-10230, which allows unauthenticated remote attackers to execute arbitrary commands on affected systems. Below is a summary of the issue and recommended actions."

---

## Required Sections (in order)

### 1. Background
Explain *what* the vulnerability or security event is. Keep it factual and clear:
- What product/component is affected
- Root cause of the vulnerability (e.g., improper input sanitization, deserialization flaw, auth bypass)
- Any relevant context (e.g., when it was disclosed, by whom)
- No need for bullet points — a paragraph is preferred for narrative clarity

### 2. Impact
Bullet-point format listing severity signals and consequences:
- CVSS Score (if available): e.g., `CVSS Score: 10.0 (Critical)` or `Severity: Important`
- CVE ID (repeated here for reference if complex advisory)
- Attack Vector: Remote / Local / Network
- Exploitability: e.g., "No user interaction required", "Low complexity"  
- Consequences: nested bullets (full system compromise, data theft, ransomware deployment, etc.)

### 3. Attack Observation
Describe what is actually happening / being observed in the wild:
- Is there active exploitation? PoC available?
- What does the attack look like technically (crafted requests, lure mechanisms, TTPs)
- Include MITRE ATT&CK technique IDs where relevant (e.g., T1003.001)
- If no in-the-wild exploitation: say so clearly — "As of [date], no confirmed exploitation has been reported."

### 4. Affected Versions / Affected Environments
- List specific version ranges in bullet format
- For software: use exact version numbers (e.g., `4.0 through 4.21.8`)
- For cloud services / managed services: describe the exposure condition
- For technique-driven threats (no CVE): describe what environment types are at risk
- May use a table format when there are multiple products

### 5. Remediation
- Concrete, actionable steps
- Distinguish between patches (primary fix) and workarounds (temporary mitigations)
- Tag each action with `[Client]` or `[EnSOC]` prefix to assign responsibility:
  - `[Client]` = action the client must take
  - `[EnSOC]` = action Ensign SOC will perform / monitor for
- Include official vendor advisory links where available

### 6. Recommendations
Additional strategic or operational guidance beyond the immediate remediation:
- Hardening measures
- Monitoring rules
- User awareness
- Also tagged with `[Client]` or `[EnSOC]`

> Note: Some emails merge Remediation and Recommendations into a single section titled "Remediation & Recommendation" — both formats are acceptable.

---

## Writing Style Guidelines

### Tone
- Professional but not overly stiff — readable and urgent when warranted
- Direct and factual; avoid marketing language or excessive hedging
- Balanced: communicate risk clearly without causing panic

### Language Patterns
- Use active voice: "Attackers can execute..." not "Code execution may be possible..."
- State exploitation status clearly: "No confirmed exploitation in the wild" vs "Actively exploited since [date]"
- Use precise language: "unauthenticated remote attacker" not "hackers"
- Numbers and scores should be exact: CVSS 9.1, not "very high"

### IOC Presentation
When IOCs are available, include them as a bulleted list

**Format for IOC bullet points:**
- **[Type]**: [defanged indicator] - [description]
- **IP:Port**: 176.120.22[.]127:80 - C2 server
- **Filename**: z1.bat - Pre-deployment script
- **Email**: cloud-init@mail.io - Malicious account

- Always defang IPs: use `[.]` notation (e.g., `104.28.244[.]115`)
- If no IOCs are available or relevant, omit this section

### MITRE ATT&CK References
- Include technique IDs when describing observed TTPs in Attack Observation
- Format: `credential dumping (T1003.001)`, `service stop (T1489)`

### Vulnerability vs. Threat Event Distinction
The advisory covers two main scenario types:

**CVE/Vulnerability advisories**: Focus on the flaw, affected versions, and patch urgency
**Threat campaign / security event advisories**: Focus on the threat actor, TTPs, IOCs, and detection logic

Adjust emphasis accordingly — campaign advisories need more IOC and TTP detail.

---

## Quality Checklist (Self-review before finalizing)

Before sending, verify the advisory answers these questions for the client:

- [ ] **What happened?** — Is the Background section clear and factual?  
- [ ] **How bad is it?** — Is the severity/CVSS communicated? Are the worst-case consequences stated?
- [ ] **Is it being exploited?** — Is exploitation status explicitly stated (yes/no/PoC available)?
- [ ] **Am I affected?** — Are the affected versions/configurations listed clearly?
- [ ] **What do I need to do?** — Are client actions clearly tagged with `[Client]`?
- [ ] **What will Ensign do?** — Are Ensign SOC monitoring actions tagged with `[EnSOC]`?
- [ ] **Are IOCs included?** — If exploitation is observed, are network/file indicators provided?
- [ ] **Are sources cited?** — Are vendor advisories, CVE links, or research sources referenced?
- [ ] **Is the tone appropriate?** — Is urgency proportional to actual severity?
- [ ] **Is it complete?** — No placeholder text like "[TBD]" or empty sections?

---

## Advisory Type Quick Reference

| Scenario                       | Subject Prefix                          | Key Sections to Emphasize                    | IOC Priority           |
| ------------------------------ | --------------------------------------- | -------------------------------------------- | ---------------------- |
| CVE with patch available       | `CVE-XXXX-XXXXX – [Product]`            | Affected Versions, Remediation               | Low (unless exploited) |
| CVE actively exploited (0-day) | `CVE-XXXX: Actively Exploited Zero-Day` | Attack Observation, IOCs, urgent Remediation | High                   |
| Ransomware campaign            | `[Threat Group] Ransomware Activity`    | TTPs, IOCs, Affected Environments            | Critical               |
| Threat toolkit exposed         | `Exposed [Name] Toolkit`                | IOCs, Attack Observation, Recommendations    | Critical               |
| AI/emerging threat             | `[Risk Type] in [Product/Technology]`   | Background, Impact, Recommendations          | Medium                 |
