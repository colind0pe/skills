# Security Advisory Research Guide

Detailed guidance for Phase 2 of the advisory workflow: gathering intelligence and verifying it before drafting.

---

## Information Sources by Advisory Type

### CVE / Vulnerability Advisories

Check these in priority order:

1. **NVD** (nvd.nist.gov) — official CVSS score, CWE classification, affected product list
2. **Vendor advisory** — the authoritative source for affected versions and official patches (e.g., Microsoft MSRC, Fortinet PSIRT, Cisco Talos, Palo Alto Unit 42)
3. **Security research blogs** — BleepingComputer, The Hacker News, Securelist, Rapid7, Tenable, Qualys, Mandiant — for in-the-wild exploitation evidence and technical analysis
4. **Threat intelligence sources** — CISA KEV (Known Exploited Vulnerabilities catalog), MITRE ATT&CK, VirusTotal

### Threat Campaigns / Security Events (no CVE)

Search for:

- Threat actor attribution reports from major vendors (CrowdStrike, Mandiant, Recorded Future, Sophos)
- IOC lists from threat intelligence feeds
- MITRE ATT&CK technique mappings used in the campaign
- Confirmed victims, sectors targeted, or geographic focus
- Any exposed toolkits, malware families, or TTPs documented in sandbox analyses

---

## Handling Provided URLs

If the user provides a specific URL (blog post, news article, vendor advisory):

1. **Read it first** using `read_url_content` as your baseline reference
2. **Still perform web searches** — do not rely solely on a single source; cross-reference to build a complete picture
3. Note any discrepancies between the provided URL and other sources

---

## Relevance Verification

When gathering search results, actively filter out unrelated articles. Use these signals to confirm a source is about the same event:

- **CVE match**: The exact CVE ID appears in the article (not just a similar CVE number)
- **Product match**: The affected product name and version range align with what other sources say
- **Timeline alignment**: The article date is close to the disclosure or exploitation date
- **Cross-source consistency**: Multiple independent sources describe the same technical details

If results return mixed or ambiguous information — e.g., a CVE number used in multiple contexts, or a vendor with multiple recent advisories — pick the most authoritative source and note any ambiguity:

> **Priority order**: Official vendor advisory > NVD > reputable security research blogs > news media

---

## Intelligence Checklist

Before moving to the draft phase, confirm you have collected:

- [ ] Severity / CVSS score (exact number preferred)
- [ ] Affected product(s) and specific version ranges
- [ ] Root cause of the vulnerability or nature of the threat
- [ ] Exploitation status: actively exploited in the wild? PoC publicly available? No confirmed exploitation?
- [ ] Patch / fix information: fixed versions, vendor advisory link
- [ ] IOCs (IP addresses, file hashes, domain names, email addresses) — if applicable to the event type
- [ ] MITRE ATT&CK TTPs — especially important for campaign advisories

If a critical piece of information (particularly exploit status or affected versions) cannot be found after thorough searching, note the gap explicitly in the advisory rather than guessing.

---

## Tool Usage Note

If the `tavily_search` tool is available, prioritize it over `search_web` — it provides enhanced research capabilities suited to security intelligence gathering.
