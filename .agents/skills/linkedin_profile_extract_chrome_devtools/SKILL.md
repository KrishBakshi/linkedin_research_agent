---
name: linkedin-profile-extract-chrome-devtools
description: "Extracts structured data from a single LinkedIn person profile URL using the chrome-devtools MCP server. Use when the user provides a LinkedIn profile URL and wants: name, current company, current role, profile headline, location, linkedin URL, company URL, and public contact info (email, websites, phone, etc)."
---

# LinkedIn Profile Extractor (Chrome DevTools)

Use this skill to extract one person profile from a provided LinkedIn URL using `mcp__chrome-devtools__*` tools.

## Required Input

- A LinkedIn person URL in the format `https://www.linkedin.com/in/<handle>/`

## Workflow

1. Open the profile URL with `mcp__chrome-devtools__new_page`.
2. Take a snapshot with `mcp__chrome-devtools__take_snapshot`.
3. Extract top-card fields from visible profile content:
- `name`
- `profile_headline`
- `location`
- `linkedin_url` (canonical profile URL)
4. Extract current role and company from the Experience section:
- Use the first listed active role (`Present`) as `current_role` and `current_company`.
- If multiple concurrent roles exist, return the role that matches the profile headline first; otherwise return the first `Present` role.
5. Resolve `company_url`:
- Prefer the company link attached to the selected current role in Experience.
- If not available, return `null`.
6. Open `Contact info` and extract public contact fields only.
- Include only visible/public items (for example: email, website, phone, address, birthday, twitter, etc.).
- Do not infer hidden values.
7. Close the contact modal and provide normalized output.

## Output Format

Return exactly this JSON shape:

```json
{
  "name": "string | null",
  "current_company": "string | null",
  "current_role": "string | null",
  "profile_headline": "string | null",
  "location": "string | null",
  "linkedin_url": "string | null",
  "company_url": "string | null",
  "public_contact_info": {
    "emails": ["string"],
    "websites": ["string"],
    "phones": ["string"],
    "other": [
      {
        "type": "string",
        "value": "string"
      }
    ]
  }
}
```

## Quality Rules

- Use only data visible in the page/overlay.
- Prefer `null` over guessing.
- Do not open unrelated profiles.
- Keep extraction to the requested fields only.

## Contact Info Guardrails (Important)

- Always open the contact overlay URL directly when possible:
  - `https://www.linkedin.com/in/<handle>/overlay/contact-info/`
- Parse contact data only from the overlay dialog container (`dialog` / `[role="dialog"]`), never from the full page body.
- Ignore global page links/navigation/footer links outside the dialog.
- For `websites`, exclude:
  - LinkedIn profile URLs (`linkedin.com/in/...`)
  - Premium upsell URLs (`linkedin.com/premium/products...`)
- If LinkedIn returns redirect wrappers (`linkedin.com/redir/redirect/?url=...`), decode and store the destination URL.
- Treat numeric fragments from profile slugs (for example IDs in profile URL) as non-phone noise.
- If contact fields are not visible in the dialog, return empty arrays rather than inferring values.
