# LinkedIn Research Agent

This repo defines a Codex/Gemini-style LinkedIn sourcing agent that can:

1. Build LinkedIn Boolean queries
2. Run LinkedIn People searches in Comet browser
3. Collect profile URLs from search results
4. Optionally extract structured data from individual profiles

## What Is In This Repo

- `AGENT.md`: Primary operating instructions for the agent workflow
- `.agents/skills/linkedin_bollean_search/SKILL.md`: Boolean query construction rules
- `.agents/skills/linkedin_search_page_naviagtion/SKILL.md`: LinkedIn search navigation + URL collection
- `.agents/skills/linkedin_profile_extract_chrome_devtools/SKILL.md`: Single-profile structured extraction rules
- `runs/`: Output directory for timestamped JSON results

## Prerequisites

- Codex-compatible environment (or equivalent agent runner)
- Access to a logged-in LinkedIn account
- Node.js/npm (for MCP servers that run via `npx`)
- MCP servers configured in `.codex/config.toml`:
  - `comet-mcp`
  - `chrome-devtools`
  - `notion` (optional)

## MCP Configuration

The repo already includes MCP config in `.codex/config.toml`:

```toml
[mcp_servers.comet-mcp]
command = "npx"
args = ["-y", "comet-mcp"]

[mcp_servers.notion]
url = "https://mcp.notion.com/mcp"

[mcp_servers.chrome-devtools]
command = "npx"
args = ["chrome-devtools-mcp@latest", "--userDataDir=/Users/nasiwakmac/.cache/chrome-devtools-mcp/chrome-profile"]
```

Update paths as needed for your machine.

## How To Use

### 1) Start the agent in this repo

Run your Codex/agent session from the repository root so it can load `AGENT.md` and the local skills.

### 2) Give a sourcing request

Example prompts:

- `Find AI engineers based in Japan.`
- `Find senior software engineers with AWS or Azure experience, not managers.`
- `Find founders associated with Japan in AI.`

The agent should do this flow:

1. Parse role, skills, exclusions, and location intent
2. Build a LinkedIn Boolean query
3. Run LinkedIn search in Comet
4. Click `People` filter and any requested filters
5. Return only profile URLs

### 3) (Optional) Extract profile data

If you provide one or more LinkedIn profile URLs and ask for details, the agent should use the Chrome DevTools extraction skill to return:

- Name
- Current company
- Current role
- Headline
- Location
- LinkedIn URL
- Company URL
- Public contact info

## Output Rules (Critical)

When collecting URLs with Comet, output must be plain URLs only, one per line, with no bullets or labels.

Correct format:

```text
https://www.linkedin.com/in/username1/
https://www.linkedin.com/in/username2/
https://www.linkedin.com/in/username3/
```

## Results Storage

Structured JSON outputs are stored in `runs/` using this format:

`linkedin_results_YYYY-MM-DD_HH-MM-SS.json`

Example:

`runs/linkedin_results_2026-02-06_14-30-45.json`

`runs/*.json` is git-ignored.

## Best Practices

- Keep Boolean queries compact (typically 2-4 `OR` terms per group)
- Use short forms where applicable (`ai`, `ml`)
- Use quotes for multi-word titles (`"product manager"`)
- Use LinkedIn filters for explicit constraints (location, company, connections)
- Do not open profiles in Comet during URL collection
- Use Chrome DevTools skill for profile extraction, not Comet

## Troubleshooting

- No results: simplify query and reduce `OR` complexity
- Wrong audience: verify filter usage (`based in` uses location filter; `associated with` is often a keyword)
- Browser failures: verify LinkedIn session is logged in and MCP server is connected
- Missing contact fields: return empty values instead of guessing

## Responsible Use

- **This project is for educational and knowledge purposes only.**
- **Do not use it to violate LinkedIn terms, platform guidelines, privacy laws, or any applicable regulations.**
- **Exploiting or misusing this project can lead to account restrictions or bans.**
- **Each individual user is fully responsible for their actions, not this project or any contributor to it.**
