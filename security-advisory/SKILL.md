---
name: security-advisory
description: Research a new vulnerability or security event and generate a professional Security Advisory email ready to send to clients. Use this skill whenever the user mentions a CVE number, vulnerability name, ransomware campaign, threat actor activity, or any security event they want to communicate to clients. Trigger on phrases like "write a security advisory", "draft an advisory for CVE-XXXX", "send advisory about [threat]", "generate advisory", or any request to inform clients about a current threat — even if they don't say "advisory" explicitly. Always use this skill when the user provides a CVE ID or vulnerability name and wants a client-facing email.
---

# Security Advisory Skill

This skill helps you research a newly disclosed vulnerability or security event, then draft a professional Security Advisory email for clients — matching the style, structure, and quality of Ensign InfoSecurity's existing advisories.

The full advisory template and style guide is in `references/advisory-guideline.md`. Read it before drafting.

---

## Workflow Overview

Follow these four phases in order:

1. **Understand the input**
2. **Research and verify**
3. **Draft the advisory**
4. **Self-review and finalize**

---

## Phase 1: Understand the Input

The user will provide a vulnerability or security event. Extract:

- **Identifier**: CVE ID (e.g., CVE-2025-12345), vendor advisory ID, threat actor name, or event description
- **Type**: Is this a CVE/vulnerability advisory or a threat campaign/security event advisory?
- **Context clues**: Any initial severity signals, affected products, or exploitation status already known

If the input is ambiguous (e.g., just a CVE number with no context), proceed to research — don't ask the user before looking it up.

---

## Phase 2: Research and Verify

### Step 2a: Gather information from multiple sources

Search for the vulnerability or security event using the `search_web` tool. For CVEs, check:

1. NVD (nvd.nist.gov) for official CVSS score, description, and CWE
2. Vendor advisory (e.g., Microsoft, Fortinet, Cisco, Palo Alto)
3. Security research blogs (e.g., BleepingComputer, The Hacker News, Securelist, Rapid7, Tenable, Qualys, Mandiant)
4. Threat intelligence sources (e.g., CISA KEV, MITRE ATT&CK, VirusTotal)

For threat campaigns or security events (no CVE), search for:
- Threat actor reports from major vendors
- IOC lists from threat intelligence feeds
- MITRE ATT&CK technique mappings
- Any confirmed victims or sectors targeted

### Step 2b: Verify this is the same event

When gathering results, actively filter out unrelated articles. Use these signals to confirm relevance:

- **CVE match**: The exact CVE ID appears in the article (not just a similar CVE)
- **Product match**: The affected product name and version range align
- **Timeline alignment**: The article date is close to the disclosure or exploitation date
- **Cross-source consistency**: Multiple independent sources describe the same technical details

If results return mixed or ambiguous information (e.g., a CVE number used in multiple contexts, or a vendor name with multiple recent advisories), note this explicitly and pick the most authoritative source (vendor official > NVD > reputable security blogs).

### Step 2c: Collect what you need

Before drafting, confirm you have:
- [ ] Severity / CVSS score
- [ ] Affected product(s) and version range(s)
- [ ] Root cause of the vulnerability or nature of the threat
- [ ] Exploitation status (in-the-wild? PoC available? No confirmed exploitation?)
- [ ] Patch/fix information (fixed versions, vendor advisory link)
- [ ] IOCs (IP addresses, file hashes, domain names, email addresses) — if applicable
- [ ] MITRE ATT&CK TTPs — if a campaign advisory

If a critical piece of information (especially exploit status or affected versions) is not found, note the gap explicitly in the advisory rather than guessing.

---

## Phase 3: Draft the Advisory

Read `references/advisory-guideline.md` for the style guide and quality checklist. Then, use the `view_file` tool to read the strict layout template from `assets/advisory-template.md` and draft your content into that exact structure.

---

## Phase 4: Self-Review and Refinement

After drafting, pause and critically evaluate the advisory before presenting it.

Use the **Quality Checklist** found at the bottom of `references/advisory-guideline.md`. Specifically perform self-correction for common errors before showing the user:
- Missing exploitation status → explicitly search and add it
- Vague remediation → make it specific (exact versions, exact commands)
- Missing `[Client]` and `[EnSOC]` tags → assign them clearly
- Undefanged IOCs → defang them all (e.g., `127.0.0[.]1`)

If you identify any gaps, fix them before outputting your final response.

### Final output format
After the advisory, include the `## References` section as shown in `assets/advisory-template.md`. You MUST populate this section with the actual URLs and source names you found during your research in Phase 2. Do NOT output placeholders like `[URL]` or ask the user to fill them in.

Always cite the real, functioning links for: the official vendor advisory, at least one secondary source (news or research), and the NVD/CVE entry if applicable.


