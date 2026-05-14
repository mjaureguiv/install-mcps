# install-mcps

Interactively install essential MCP servers (Databricks, Productboard, Atlassian, Microsoft 365, SAP Auth, SAP Wiki, SAP KBA, SAP Jira, SAP Help, Playwright, Figma, SAP Fiori, Context7, CAP, GitHub, SAP GitHub Tools, SAP WDF GitHub) with guided credential collection.

## What It Does

- Checks which MCPs are already installed
- Installs local MCP tools via `uv tool install`
- Collects credentials interactively via AskUserQuestion
- Adds MCPs to Claude Code with user scope
- Verifies each installation

## Supported MCPs

| MCP | Source | Auth | What It Enables |
|-----|--------|------|-----------------|
| **Databricks** | [databricks-mcp](https://github.tools.sap/I529122/databricks-mcp) | OAuth browser | SQL queries against Unity Catalog |
| **Productboard** | [Hosted SSE](https://leanix-productboard-mcp.internal.cfapps.eu12.hana.ondemand.com) | VPN only | Feature and notes management |
| **Atlassian** | Hosted service | OAuth browser | Jira and Confluence integration |
| **Microsoft 365** | [microsoft-365-mcp](https://github.tools.sap/LeanIX/microsoft-365-mcp) | Device code | Calendar, email, tasks, and Teams via Microsoft Graph |
| **SAP Auth** | [sap-auth-mcp](https://github.tools.sap/I529243/sap-auth-mcp) | Browser SSO | Centralized SAP SSO authentication for other SAP MCPs |
| **SAP Wiki** | [sap-wiki-mcp](https://github.tools.sap/sfsfmcp/sap-wiki-mcp) | Cookie (via SAP Auth) | Search and fetch content from SAP Wiki/Confluence |
| **SAP KBA** | [sap-kba-mcp](https://github.tools.sap/I760189/sap-kba-mcp) | Browser (Selenium) | Search SAP internal Knowledge Base Articles and Notes |
| **SAP Jira** | [sap-jira-mcp](https://github.tools.sap/sfsfmcp/sap-jira-mcp) | Cookie (via SAP Auth) | Search and manage tickets on SAP's internal Jira |
| **SAP Help** | [sap-help-mcp](https://github.tools.sap/I529243/sap-help-mcp) | None (open APIs) | Search and retrieve documentation across all SAP products on help.sap.com |
| **Playwright** | [@anthropic-ai/playwright-mcp](https://www.npmjs.com/package/@anthropic-ai/playwright-mcp) | None | Browser automation for screenshots, web scraping, and UI testing |
| **Figma** | [Figma MCP](https://github.com/figma/mcp-server-guide) | OAuth browser | Design file integration for AI code generation from Figma |
| **SAP Fiori** | [fiori-mcp-server](https://github.com/SAP/open-ux-tools/tree/main/packages/fiori-mcp-server) | None | SAP Fiori tools for UI5 app development, Fiori elements, and freestyle apps |
| **Context7** | [context7](https://github.com/upstash/context7) | None | Up-to-date library documentation and code examples for any framework |
| **CAP** | [cap-js/mcp-server](https://github.com/cap-js/mcp-server) | None | AI-assisted CAP/CDS development with model search and documentation |
| **GitHub** | [github-mcp-server](https://github.com/github/github-mcp-server) | PAT | Repository, issues, PRs, and Actions management on github.com |
| **SAP GitHub Tools** | [tools-github-mcp](https://github.tools.sap/github/tools-github-mcp) | PAT | Repository management on SAP internal GitHub (github.tools.sap) |
| **SAP WDF GitHub** | [github-wdf-mcp-server](https://github.tools.sap/sap.tools.hyperspace/github-wdf-mcp-server) | PAT | Repository management on SAP WDF GitHub instance |

## Prerequisites

### Required Credentials

| MCP | What You Need | Where to Get It |
|-----|---------------|-----------------|
| Databricks | Workspace URL + Warehouse ID | Azure Databricks → SQL Warehouses → Connection Details |
| Productboard | SAP VPN connection | Connect to SAP VPN before first use |
| Atlassian | Browser sign-in | Automatic OAuth flow on first use |
| Microsoft 365 | Browser sign-in | Run `microsoft-365-mcp-auth` during install |
| SAP Auth | SAP email + Node.js 20+ + browser | Browser SSO on first use |
| SAP Wiki | SAP Auth MCP installed | Authentication handled by SAP Auth MCP |
| SAP KBA | Python 3.10+ + uv + Chrome/Chromium | Built-in browser auth on first use |
| SAP Jira | SAP Auth MCP installed + Node.js 20+ | Authentication handled by SAP Auth MCP |
| SAP Help | Node.js 20+ | No authentication needed (open APIs) |
| Playwright | Node.js | No authentication needed |
| Figma | Browser sign-in | Automatic OAuth flow on first use |
| SAP Fiori | Node.js | No authentication needed |
| Context7 | Node.js | No authentication needed |
| CAP | Node.js | No authentication needed |
| GitHub | Personal Access Token | https://github.com/settings/personal-access-tokens/new |
| SAP GitHub Tools | Personal Access Token | https://github.tools.sap/settings/tokens |
| SAP WDF GitHub | Personal Access Token | SAP WDF GitHub settings |

## Installation

Copy this folder to your `.claude/skills/` directory:

```bash
cp -r install-mcps ~/.claude/skills/
```

Or install the slash command directly:

```bash
cp install-mcps/SKILL.md ~/.claude/commands/install-mcps.md
```

## Usage

```bash
/install-mcps
```

The command will:
1. Check current MCP installation status
2. Skip any MCPs already installed
3. Prompt for credentials for each missing MCP
4. Install and verify each MCP
5. Display final summary

## What's Included

```
install-mcps/
├── SKILL.md    # Slash command (copy to ~/.claude/commands/install-mcps.md)
└── README.md   # This file
```

## Example Output

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► STATUS CHECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Current installation status:
  [✗] Databricks MCP
  [✗] Productboard MCP
  [✓] Atlassian MCP
  [✗] Microsoft 365 MCP
  [✗] SAP Auth MCP
  [✗] SAP Wiki MCP
  [✗] SAP KBA MCP
  [✗] SAP Jira MCP
  [✗] SAP Help MCP
  [✗] Playwright MCP
  [✗] Figma MCP
  [✗] SAP Fiori MCP
  [✗] Context7 MCP
  [✗] CAP MCP
  [✗] GitHub MCP
  [✗] SAP GitHub Tools MCP
  [✗] SAP WDF GitHub MCP

Installing missing MCPs...
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "Command not found: uv" | Install uv: `curl -LsSf https://astral.sh/uv/install.sh \| sh` |
| "MCP already exists" | The command will skip already-installed MCPs |
| OAuth not working | Delete `~/.databricks/token-cache.json` and retry |
| MCP shows disconnected | Restart Claude Code after installation |

## After Installation

1. **Restart Claude Code** to load the new MCPs
2. Run `claude mcp list` to verify all connections
3. For Databricks/Atlassian/Figma: sign in via browser when first prompted
4. For SAP Auth/Wiki/Jira: authentication triggers automatically via browser on first use
5. For SAP KBA: uses built-in browser auth on first use (Chrome/Chromium required)
6. SAP Help, Playwright, SAP Fiori, Context7, and CAP work immediately (no auth needed)
7. For GitHub MCPs: ensure your Personal Access Tokens have correct scopes (repo, issues, pull_request)

## Related Skills

- **customer-briefing**: Uses Databricks + Amplitude + Productboard for customer prep
- **productboard-feedback**: Uses Productboard MCP for feedback processing
- **user-docs**: Uses Atlassian MCP for Confluence publishing
