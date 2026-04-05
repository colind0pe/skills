---
name: security-advisory
description: Draft a professional, client-facing Security Advisory email about a newly disclosed vulnerability (CVE) or security event. Use this skill ONLY when the user explicitly requests to write, draft, or generate a security advisory. Trigger on phrases like "write a security advisory", "draft an advisory for CVE-XXXX", "generate an advisory about [threat]", or any clear request for a client-facing alert. Do NOT trigger if the user is only asking for general information about a CVE without requesting to write an advisory.
---

# Security Advisory Skill

This skill researches a newly disclosed vulnerability or security event, then drafts a professional client-facing Security Advisory email that matches company's established style and quality.

**Reference files:**
- `references/writing-guide.md` — writing style, section requirements, quality checklist, and advisory type quick-reference
- `references/research-guide.md` — intelligence sources, relevance verification, and pre-draft checklist
- `assets/advisory-template.md` — the exact output template to populate

---

## Phase 1: Understand the Input

Extract from the user's request:

- **Identifier** — CVE ID, vendor advisory ID, threat actor name, or event description
- **Advisory type** — CVE/vulnerability advisory, or threat campaign/security event advisory
- **Provided URLs** — any links the user included as a starting reference

If the input is ambiguous (e.g., just a CVE number), proceed directly to research.

---

## Phase 2: Research and Verify

Read `references/research-guide.md` for the full list of sources, relevance verification criteria, and the intelligence checklist to complete before drafting.

The goal: gather enough verified facts to populate every section of the advisory with real, confirmed data — no guessing, no placeholders.

---

## Phase 3: Draft the Advisory

Read `references/writing-guide.md` for style, tone, and section-level writing guidance.  
Then read `assets/advisory-template.md` and draft your content directly into that structure.

---

## Phase 4: Self-Review and Finalize

Run through the **Quality Checklist** at the bottom of `references/writing-guide.md`. Fix any gaps before presenting the output — don't leave self-correction to the user.

These are the most frequent mistakes — catch them first, then run the full checklist:
- Exploitation status not explicitly stated → search and add it
- Remediation steps are vague → make them specific (exact versions, exact commands)
- Missing `[Client]` / `[EnSOC]` action tags → assign them clearly
- IOCs not defanged → apply `[.]` notation (e.g., `104.28.244[.]115`)
- References section uses placeholders → populate with actual URLs collected during research
