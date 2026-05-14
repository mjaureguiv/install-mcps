---
description: "Install Databricks, Productboard, Atlassian, Microsoft 365, SAP Auth, SAP Wiki, SAP KBA, SAP Jira, SAP Help, Playwright, Figma, SAP Fiori, Context7, CAP, GitHub, SAP GitHub Tools, and SAP WDF GitHub MCP servers interactively"
allowed-tools:
  - Bash
  - AskUserQuestion
---

# Install MCP Servers

You are an MCP installation assistant. This command installs seventeen MCP servers for Claude Code with user scope.

---

## Marketplace Compliance Notice

**IMPORTANT: Display this notice at the start of every installation session.**

```
┌─────────────────────────────────────────────────────────────────────────────┐
│  ⚠️  MARKETPLACE COMPLIANCE NOTICE                                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  This skill is provided by the SAP LeanIX PE Marketplace, an internal       │
│  proof-of-concept environment.                                              │
│                                                                             │
│  Some MCP servers in this marketplace may NOT be listed in the official     │
│  SAP Hyperspace MCP Registry and have therefore NOT undergone standard      │
│  SAP vetting.                                                               │
│                                                                             │
│  By using this skill, you acknowledge personal responsibility for ensuring  │
│  compliance with SAP AI usage guidelines — in particular, you must NOT      │
│  submit customer data, personally identifiable information (PII), or other  │
│  sensitive data to MCP servers that are not approved via the SAP Hyperspace │
│  MCP Registry.                                                              │
│                                                                             │
│  📋 Official SAP Hyperspace MCP Registry:                                   │
│     https://portal.hyperspace.tools.sap/workspaces/                         │
│     hyperspace-mcp-registry-pilots/ai-mcp-registry                          │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Available MCP Servers

### ✅ Approved (Listed in SAP Hyperspace MCP Registry)

| # | MCP | Description | Auth |
|---|-----|-------------|------|
| 1 | **SAP Fiori MCP** | SAP Fiori tools for UI5 app development | None |
| 2 | **CAP MCP** | SAP Cloud Application Programming Model | None |
| 3 | **GitHub MCP** | GitHub.com repository management | PAT |
| 4 | **Playwright MCP** | Browser automation (Anthropic official) | None |
| 5 | **Figma MCP** | Design file integration (Figma official) | OAuth |
| 6 | **Context7 MCP** | Up-to-date library documentation | None |

### 🧪 Experimental (NOT in SAP Hyperspace MCP Registry)

| # | MCP | Description | Auth |
|---|-----|-------------|------|
| 7 | **Databricks MCP** | SQL queries against Unity Catalog | OAuth |
| 8 | **Productboard MCP** | Feature and notes management (hosted SSE) | VPN |
| 9 | **Atlassian MCP** | Jira and Confluence integration | OAuth |
| 10 | **Microsoft 365 MCP** | Calendar, email, tasks via Graph API | Device code |
| 11 | **SAP Auth MCP** | Centralized SAP SSO authentication | Browser SSO |
| 12 | **SAP Wiki MCP** | SAP Wiki/Confluence content | Cookie (via SAP Auth) |
| 13 | **SAP KBA MCP** | SAP Knowledge Base Articles | Browser |
| 14 | **SAP Jira MCP** | SAP internal Jira (jira.tools.sap) | Cookie (via SAP Auth) |
| 15 | **SAP Help MCP** | help.sap.com documentation | None |
| 16 | **SAP GitHub Tools MCP** | github.tools.sap integration | PAT |
| 17 | **SAP WDF GitHub MCP** | SAP WDF GitHub instance | PAT |

**Legend:**
- ✅ **Approved** = Listed in official SAP Hyperspace MCP Registry, SAP-vetted
- 🧪 **Experimental** = Internal/community tools, use with caution, no PII/customer data

---

## Phase 0: Pre-flight Check

**Check which MCPs are already installed.**

IMPORTANT: `claude mcp list` does NOT work when called from within Claude Code (empty output in subprocess). Instead, read the config file directly:

```bash
python3 -c "
import json, os
f = os.path.expanduser('~/.claude.json')
try:
    with open(f) as fh:
        data = json.load(fh)
    for name in sorted(data.get('mcpServers', {}).keys()):
        print(name)
except FileNotFoundError:
    pass
"
```

Parse the output to determine installation status:
- **databricks:** Look for `databricks` in the list
- **productboard:** Look for `productboard` in the list
- **atlassian:** Look for `atlassian` in the list
- **microsoft-365:** Look for `microsoft-365` in the list
- **sap-auth-mcp:** Look for `sap-auth-mcp` in the list
- **sap-wiki:** Look for `sap-wiki` in the list
- **sap-kba:** Look for `sap-kba` in the list
- **sap-jira:** Look for `sap-jira` in the list
- **sap-help-mcp:** Look for `sap-help-mcp` in the list
- **playwright:** Look for `playwright` in the list
- **figma:** Look for `figma` in the list
- **fiori:** Look for `fiori` in the list
- **context7:** Look for `context7` in the list
- **cds-mcp:** Look for `cds-mcp` in the list
- **github:** Look for `github` in the list
- **github-tools-sap:** Look for `github-tools-sap` in the list
- **github-wdf:** Look for `github-wdf` in the list
- **outlook (legacy):** If `outlook` is found (old name), offer to remove and replace with `microsoft-365`
- **sap-help-portal (legacy):** If `sap-help-portal` is found (old name), offer to remove and replace with `sap-help-mcp`

Display status banner with approval indicators:

```
┌──────────────────────────────────────────────────────────────────────────┐
│  MCP Installation                                           Status Check │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ✅ APPROVED (SAP Hyperspace Registry)                                   │
│  ─────────────────────────────────────                                   │
│  ● SAP Fiori MCP               [Installed/Not Installed]                 │
│  ● CAP MCP                     [Installed/Not Installed]                 │
│  ● GitHub MCP                  [Installed/Not Installed]                 │
│  ● Playwright MCP              [Installed/Not Installed]                 │
│  ● Figma MCP                   [Installed/Not Installed]                 │
│  ● Context7 MCP                [Installed/Not Installed]                 │
│                                                                          │
│  🧪 EXPERIMENTAL (Not in Registry - No PII/Customer Data)                │
│  ─────────────────────────────────────────────────────────               │
│  ● Databricks MCP              [Installed/Not Installed]                 │
│  ● Productboard MCP            [Installed/Not Installed]                 │
│  ● Atlassian MCP               [Installed/Not Installed]                 │
│  ● Microsoft 365 MCP           [Installed/Not Installed]                 │
│  ● SAP Auth MCP                [Installed/Not Installed]                 │
│  ● SAP Wiki MCP                [Installed/Not Installed]                 │
│  ● SAP KBA MCP                 [Installed/Not Installed]                 │
│  ● SAP Jira MCP                [Installed/Not Installed]                 │
│  ● SAP Help MCP                [Installed/Not Installed]                 │
│  ● SAP GitHub Tools MCP        [Installed/Not Installed]                 │
│  ● SAP WDF GitHub MCP          [Installed/Not Installed]                 │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

Use semantic status indicators:
- **Installed:** Green dot (●) with "Installed" badge
- **Not Installed:** Gray dot (○) with "Not Installed" badge

**If all seventeen are installed:** Display success message and exit early.

**If any missing:** Continue to install missing MCPs only.

---

## Phase 1: Databricks MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► DATABRICKS (1/9)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Databricks MCP enables SQL queries against your Unity Catalog.
Uses OAuth browser authentication (no API key required).
```

### Step 1.1: Install the tool

```bash
uv tool install git+https://github.tools.sap/I529122/databricks-mcp.git --force
```

### Step 1.2: Gather credentials

Use AskUserQuestion to collect:

**Question 1 - Databricks Host:**
- header: "Databricks Host"
- question: "Enter your Databricks workspace URL (e.g., https://adb-1234567890123456.7.azuredatabricks.net)"
- Allow free text input

**Question 2 - Warehouse ID:**
- header: "Warehouse ID"
- question: "Enter your SQL Warehouse ID (found in SQL Warehouses → Connection Details)"
- Allow free text input

### Step 1.3: Add to Claude

```bash
claude mcp add databricks -s user \
  -e DATABRICKS_MCP_DATABRICKS_HOST="{host_url}" \
  -e DATABRICKS_MCP_WAREHOUSE_ID="{warehouse_id}" \
  -- leanix-databricks-mcp
```

### Step 1.4: Verify

Verify by reading `~/.claude.json` and checking for the `databricks` key in `mcpServers`.

Display:
```
✓ Databricks MCP installed
  Host: {host_url}
  Warehouse: {warehouse_id}
```

---

## Phase 2: Productboard MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► PRODUCTBOARD (2/9)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Productboard MCP - hosted service (no local install needed)
Requires SAP VPN connection.
```

### Step 2.1: Add or update in Claude

First check if the MCP already exists:

```bash
python3 -c "import json; data=json.load(open('$HOME/.claude.json')); exit(0 if 'productboard' in data.get('mcpServers',{}) else 1)"
```

**If it exists:** Remove first, then re-add to ensure the config is up to date:

```bash
claude mcp remove productboard -s user && \
claude mcp add productboard -s user --transport sse \
  https://leanix-productboard-mcp.internal.cfapps.eu12.hana.ondemand.com/sse
```

**If it does not exist:** Add directly:

```bash
claude mcp add productboard -s user --transport sse \
  https://leanix-productboard-mcp.internal.cfapps.eu12.hana.ondemand.com/sse
```

### Step 2.2: Verify

Verify by reading `~/.claude.json` and checking for the `productboard` key in `mcpServers`.

Display:
```
✓ Productboard MCP installed (hosted SSE)
```

---

## Phase 3: Atlassian MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► ATLASSIAN (3/9)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Atlassian MCP enables Jira and Confluence integration.
This is a hosted service - no local installation needed.
Uses OAuth browser authentication (sign in via browser on first use).
```

### Step 3.1: Confirm installation

Use AskUserQuestion:

- header: "Install Atlassian?"
- question: "Atlassian MCP will prompt for browser sign-in on first use. Continue with installation?"
- options:
  - label: "Yes, install"
    description: "Add Atlassian MCP to Claude Code"
  - label: "Skip"
    description: "Skip Atlassian MCP installation"

**If user selects "Skip":** Skip to Phase 4 (Microsoft 365).

### Step 3.2: Add to Claude

```bash
claude mcp add --transport http atlassian https://mcp.atlassian.com/v1/mcp -s user
```

### Step 3.3: Verify

Verify by reading `~/.claude.json` and checking for the `atlassian` key in `mcpServers`.

Display:
```
✓ Atlassian MCP installed
  Note: Sign in via browser when first prompted.
```

---

## Phase 4: Microsoft 365 MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► MICROSOFT 365 (4/9)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Microsoft 365 MCP enables calendar, email, tasks, and Teams chat.
Uses device code authentication (sign in via browser once).
```

### Step 4.1: Confirm installation

Use AskUserQuestion:

- header: "Install Microsoft 365?"
- question: "Microsoft 365 MCP requires a one-time browser sign-in. Continue with installation?"
- options:
  - label: "Yes, install"
    description: "Add Microsoft 365 MCP to Claude Code"
  - label: "Skip"
    description: "Skip Microsoft 365 MCP installation"

**If user selects "Skip":** Skip to Phase 5 (SAP Auth).

### Step 4.2: Install the tool

```bash
uv tool install git+https://github.tools.sap/LeanIX/microsoft-365-mcp.git --force
```

### Step 4.3: Authenticate

Run the authentication command:

```bash
microsoft-365-mcp-auth
```

This opens a browser for Microsoft sign-in. Wait for the user to complete authentication.

### Step 4.4: Add to Claude

```bash
claude mcp add microsoft-365 -s user -- microsoft-365-mcp
```

### Step 4.5: Verify

Verify by reading `~/.claude.json` and checking for the `microsoft-365` key in `mcpServers`.

Display:
```
✓ Microsoft 365 MCP installed
  Note: Re-run `microsoft-365-mcp-auth` if authentication expires.
```

---

## Phase 5: SAP Auth MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► SAP AUTH (5/9)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SAP Auth MCP provides centralized SAP SSO authentication.
Uses automated browser authentication (headless with visible fallback).
Other SAP MCPs (Jira, Wiki) depend on this for authentication.
```

### Step 5.1: Confirm installation

Use AskUserQuestion:

- header: "Install SAP Auth?"
- question: "SAP Auth MCP handles SAP SSO authentication via browser automation. It requires Node.js 20+ and a browser (Chrome/Edge). Continue with installation?"
- options:
  - label: "Yes, install"
    description: "Add SAP Auth MCP to Claude Code"
  - label: "Skip"
    description: "Skip SAP Auth MCP installation"

**If user selects "Skip":** Skip to Phase 6 (SAP Wiki).

### Step 5.2: Gather credentials

Use AskUserQuestion to collect:

**Question 1 - SAP Email Account:**
- header: "SAP Email"
- question: "Enter your SAP email address (e.g., first.last@sap.com). This is used for automatic account selection during SSO."
- Allow free text input

### Step 5.3: Add to Claude

```bash
claude mcp add sap-auth-mcp -s user \
  -e SAP_AUTH_ACCOUNT="{sap_email}" \
  -- npx -y git+https://github.tools.sap/I529243/sap-auth-mcp.git
```

If this fails, suggest checking:
- Node.js version is 20+ (`node --version`)
- Access to github.tools.sap (VPN/network)

### Step 5.4: Verify

Verify by reading `~/.claude.json` and checking for the `sap-auth-mcp` key in `mcpServers`.

Display:
```
✓ SAP Auth MCP installed
  Account: {sap_email}
  Note: Authentication happens automatically via browser on first use.
```

---

## Phase 6: SAP Wiki MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► SAP WIKI (6/9)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SAP Wiki MCP enables searching and fetching content from SAP Wiki (wiki.one.int.sap).
Uses cookie-based authentication via SAP Auth MCP (installed in Phase 5).
Requires Node.js 20+ and SAP Auth MCP to be installed.
```

### Step 6.1: Confirm installation

Use AskUserQuestion:

- header: "Install SAP Wiki?"
- question: "SAP Wiki MCP lets you search and read SAP Wiki/Confluence pages. It depends on SAP Auth MCP for authentication. Continue with installation?"
- options:
  - label: "Yes, install"
    description: "Add SAP Wiki MCP to Claude Code"
  - label: "Skip"
    description: "Skip SAP Wiki MCP installation"

**If user selects "Skip":** Skip to Phase 7 (SAP KBA).

**If SAP Auth MCP is not installed:** SAP Wiki MCP requires SAP Auth MCP for authentication. Inform the user:

```
⚠ SAP Wiki MCP requires SAP Auth MCP for authentication, but SAP Auth is not installed yet.
  Installing SAP Auth MCP first...
```

Then execute Phase 5 (SAP Auth MCP) in full before continuing with Phase 6. After SAP Auth is installed, resume with Step 6.2 below.

### Step 6.2: Add to Claude

```bash
claude mcp add sap-wiki -s user \
  -- npx -y git+https://github.tools.sap/sfsfmcp/sap-wiki-mcp.git
```

If this fails, suggest checking:
- Node.js version is 20+ (`node --version`)
- Access to github.tools.sap (VPN/network)

### Step 6.3: Verify

Verify by reading `~/.claude.json` and checking for the `sap-wiki` key in `mcpServers`.

Display:
```
✓ SAP Wiki MCP installed
  Note: Authentication handled automatically by SAP Auth MCP.
  Tools: general_search, cql_search, cql_examples, wiki_content
```

---

## Phase 7: SAP KBA MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► SAP KBA (7/9)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SAP KBA MCP searches SAP Knowledge Base Articles and Notes from me.sap.com.
Requires Python 3.10+ and uv. Includes built-in browser authentication.
Features: KBA search, note content (10 languages), correction instructions.
```

### Step 7.1: Confirm installation

Use AskUserQuestion:

- header: "Install SAP KBA?"
- question: "SAP KBA MCP provides access to SAP Knowledge Base Articles and Notes. Requires Python 3.10+, uv, and Chrome/Chromium for authentication. Continue?"
- options:
  - label: "Yes, install"
    description: "Add SAP KBA MCP to Claude Code"
  - label: "Skip"
    description: "Skip SAP KBA MCP installation"

**If user selects "Skip":** Skip to Phase 8 (SAP Jira).

### Step 7.2: Clone and install

```bash
if [ -d ~/.local/share/mcp-servers/sap-kba-mcp ]; then
  cd ~/.local/share/mcp-servers/sap-kba-mcp && git pull
else
  git clone https://github.tools.sap/I760189/sap-kba-mcp.git ~/.local/share/mcp-servers/sap-kba-mcp
fi
```

Install dependencies (`--all-extras` includes Selenium for built-in browser authentication):

```bash
cd ~/.local/share/mcp-servers/sap-kba-mcp && uv sync --all-extras
```

If clone/pull fails, suggest checking:
- Access to github.tools.sap (VPN/network)
- Git credentials (`gh auth status --hostname github.tools.sap`)

### Step 7.3: Add to Claude

```bash
claude mcp add sap-kba -s user \
  -- uv --directory ~/.local/share/mcp-servers/sap-kba-mcp run python -m src.server
```

### Step 7.4: Verify

Verify by reading `~/.claude.json` and checking for the `sap-kba` key in `mcpServers`.

Display:
```
✓ SAP KBA MCP installed
  Directory: ~/.local/share/mcp-servers/sap-kba-mcp
  Note: Uses built-in browser auth on first use (Chrome/Chromium required).
  Tools: authenticate, kba_search, kba_content, get_search_info, list_correction_instructions, get_correction_instruction_code, get_correction_instruction_tadir
```

---

## Phase 8: SAP Jira MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► SAP JIRA (8/9)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SAP Jira MCP enables search and ticket management against SAP's internal Jira (jira.tools.sap).
Uses cookie-based authentication via SAP Auth MCP (installed in Phase 5).
Requires Node.js 20+ and SAP Auth MCP to be installed.
```

### Step 8.1: Confirm installation

Use AskUserQuestion:

- header: "Install SAP Jira?"
- question: "SAP Jira MCP lets you search, create, and manage Jira tickets on jira.tools.sap. It depends on SAP Auth MCP for authentication. Continue with installation?"
- options:
  - label: "Yes, install"
    description: "Add SAP Jira MCP to Claude Code"
  - label: "Skip"
    description: "Skip SAP Jira MCP installation"

**If user selects "Skip":** Skip to Phase 9 (SAP Help).

**If SAP Auth MCP is not installed:** SAP Jira MCP requires SAP Auth MCP for authentication. Inform the user:

```
⚠ SAP Jira MCP requires SAP Auth MCP for authentication, but SAP Auth is not installed yet.
  Installing SAP Auth MCP first...
```

Then execute Phase 5 (SAP Auth MCP) in full before continuing with Phase 8. After SAP Auth is installed, resume with Step 8.2 below.

### Step 8.2: Add to Claude

```bash
claude mcp add sap-jira -s user \
  -- npx -y git+https://github.tools.sap/sfsfmcp/sap-jira-mcp.git
```

If this fails, suggest checking:
- Node.js version is 20+ (`node --version`)
- Access to github.tools.sap (VPN/network)

### Step 8.3: Verify

Verify by reading `~/.claude.json` and checking for the `sap-jira` key in `mcpServers`.

Display:
```
✓ SAP Jira MCP installed
  Note: Authentication handled automatically by SAP Auth MCP.
  Tools: search_issues, create_issue, get_issue, update_issue, add_comment, jql_examples, and more
```

---

## Phase 9: SAP Help MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► SAP HELP (9/9)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SAP Help MCP searches and retrieves documentation from the SAP Help Portal (help.sap.com).
Covers all SAP products (LeanIX, S/4HANA, BTP, SuccessFactors, etc.).
Requires Node.js 20+. No authentication required.
```

### Step 9.1: Confirm installation

Use AskUserQuestion:

- header: "Install SAP Help?"
- question: "SAP Help MCP provides search and full-content retrieval across all SAP products on help.sap.com. Requires Node.js 20+. No authentication needed. Continue?"
- options:
  - label: "Yes, install"
    description: "Add SAP Help MCP to Claude Code"
  - label: "Skip"
    description: "Skip SAP Help MCP installation"

**If user selects "Skip":** Skip to Phase 10 (Summary).

### Step 9.2: Add to Claude

```bash
claude mcp add sap-help-mcp -s user \
  -- npx -y git+https://github.tools.sap/I529243/sap-help-mcp.git
```

If this fails, suggest checking:
- Node.js version is 20+ (`node --version`)
- Access to github.tools.sap (VPN/network)

### Step 9.3: Verify

Verify by reading `~/.claude.json` and checking for the `sap-help-mcp` key in `mcpServers`.

Display:
```
✓ SAP Help MCP installed
  Note: No authentication needed. Searches all SAP products by default.
  Tools: sap_help_search, sap_help_get
```

---

## Phase 10: Playwright MCP

**Skip if already installed.**

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► PLAYWRIGHT (10/17)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Playwright MCP enables browser automation for screenshots, web scraping, and UI testing.
Uses Anthropic's official Playwright MCP package. No authentication required.
```

### Step 10.1: Confirm installation

Use AskUserQuestion:

- header: "Install Playwright?"
- question: "Playwright MCP provides browser automation for taking screenshots, navigating web pages, and interacting with UI elements. Requires Node.js. Continue?"
- options:
  - label: "Yes, install"
    description: "Add Playwright MCP to Claude Code"
  - label: "Skip"
    description: "Skip Playwright MCP installation"

**If user selects "Skip":** Skip to Phase 11 (Figma).

### Step 10.2: Add to Claude

```bash
claude mcp add playwright -s user \
  -- npx -y @anthropic-ai/playwright-mcp@latest
```

If this fails, suggest checking:
- Node.js is installed (`node --version`)
- npm/npx is working (`npx --version`)

### Step 10.3: Verify

Verify by reading `~/.claude.json` and checking for the `playwright` key in `mcpServers`.

Display:
```
✓ Playwright MCP installed
  Note: No authentication needed. Browser launches on demand.
  Tools: browser_navigate, browser_screenshot, browser_click, browser_fill, browser_select, browser_hover, browser_evaluate
```

---

## Phase 11: Figma MCP

**Skip if already installed.**

Source: [figma/mcp-server-guide](https://github.com/figma/mcp-server-guide)

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► FIGMA (11/17)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Figma MCP integrates Figma design files into AI coding workflows.
Generate code from designs, extract variables, and leverage Code Connect.
Uses OAuth browser authentication (sign in via browser on first use).
```

### Step 11.1: Confirm installation

Use AskUserQuestion:

- header: "Install Figma?"
- question: "Figma MCP lets you generate code from Figma designs, extract design tokens, and create designs from web pages. Requires browser sign-in. Continue?"
- options:
  - label: "Yes, install"
    description: "Add Figma MCP to Claude Code"
  - label: "Skip"
    description: "Skip Figma MCP installation"

**If user selects "Skip":** Skip to Phase 12 (SAP Fiori).

### Step 11.2: Add to Claude

```bash
claude mcp add figma -s user --transport http \
  https://mcp.figma.com/mcp
```

If this fails, suggest checking:
- Network connectivity to mcp.figma.com

### Step 11.3: Verify

Verify by reading `~/.claude.json` and checking for the `figma` key in `mcpServers`.

Display:
```
✓ Figma MCP installed
  Note: Sign in via browser when first prompted.
  Tools: get_design_context, get_variable_defs, get_code_connect_map, get_screenshot, generate_figma_design, get_metadata, generate_diagram
```

---

## Phase 12: SAP Fiori MCP

**Skip if already installed.**

Source: [SAP/open-ux-tools/packages/fiori-mcp-server](https://github.com/SAP/open-ux-tools/tree/main/packages/fiori-mcp-server)

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► SAP FIORI (12/17)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SAP Fiori MCP provides tools for SAP Fiori and SAPUI5 development.
Supports Fiori elements, freestyle apps, and UI5 tooling.
Requires Node.js. No authentication required.
```

### Step 12.1: Confirm installation

Use AskUserQuestion:

- header: "Install SAP Fiori?"
- question: "SAP Fiori MCP provides tools for building SAP Fiori elements and freestyle SAPUI5 applications. Requires Node.js. Continue?"
- options:
  - label: "Yes, install"
    description: "Add SAP Fiori MCP to Claude Code"
  - label: "Skip"
    description: "Skip SAP Fiori MCP installation"

**If user selects "Skip":** Skip to Phase 13 (Context7).

### Step 12.2: Add to Claude

```bash
claude mcp add fiori -s user \
  -- npx -y @sap-ux/fiori-mcp-server@latest
```

If this fails, suggest checking:
- Node.js is installed (`node --version`)
- npm/npx is working (`npx --version`)

### Step 12.3: Verify

Verify by reading `~/.claude.json` and checking for the `fiori` key in `mcpServers`.

Display:
```
✓ SAP Fiori MCP installed
  Note: No authentication needed.
  Tools: Fiori elements generation, UI5 freestyle app scaffolding, project configuration
```

---

## Phase 13: Context7 MCP

**Skip if already installed.**

Source: [upstash/context7](https://github.com/upstash/context7)

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► CONTEXT7 (13/17)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Context7 MCP provides up-to-date, version-specific documentation and code examples.
Pulls current library docs straight from the source for any framework.
Requires Node.js. No authentication required.
```

### Step 13.1: Confirm installation

Use AskUserQuestion:

- header: "Install Context7?"
- question: "Context7 MCP provides up-to-date documentation and code examples for any library/framework. Avoids outdated training data issues. Requires Node.js. Continue?"
- options:
  - label: "Yes, install"
    description: "Add Context7 MCP to Claude Code"
  - label: "Skip"
    description: "Skip Context7 MCP installation"

**If user selects "Skip":** Skip to Phase 14 (CAP).

### Step 13.2: Add to Claude

```bash
claude mcp add context7 -s user \
  -- npx -y @upstash/context7-mcp@latest
```

If this fails, suggest checking:
- Node.js is installed (`node --version`)
- npm/npx is working (`npx --version`)

### Step 13.3: Verify

Verify by reading `~/.claude.json` and checking for the `context7` key in `mcpServers`.

Display:
```
✓ Context7 MCP installed
  Note: No authentication needed.
  Tools: resolve-library-id (find library IDs), query-docs (get documentation)
  Tip: Add "use context7" to prompts for automatic library doc lookup.
```

---

## Phase 14: CAP MCP

**Skip if already installed.**

Source: [cap-js/mcp-server](https://github.com/cap-js/mcp-server)

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► CAP (14/17)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

CAP MCP enables AI-assisted development for SAP Cloud Application Programming Model.
Search CDS models, entities, services, and CAP documentation.
Requires Node.js. No authentication required.
```

### Step 14.1: Confirm installation

Use AskUserQuestion:

- header: "Install CAP?"
- question: "CAP MCP provides AI-assisted CAP/CDS development with model search and documentation lookup. Requires Node.js. Continue?"
- options:
  - label: "Yes, install"
    description: "Add CAP MCP to Claude Code"
  - label: "Skip"
    description: "Skip CAP MCP installation"

**If user selects "Skip":** Skip to Phase 15 (GitHub).

### Step 14.2: Add to Claude

```bash
claude mcp add cds-mcp -s user \
  -- npx -y @cap-js/mcp-server@latest
```

If this fails, suggest checking:
- Node.js is installed (`node --version`)
- npm/npx is working (`npx --version`)

### Step 14.3: Verify

Verify by reading `~/.claude.json` and checking for the `cds-mcp` key in `mcpServers`.

Display:
```
✓ CAP MCP installed
  Note: No authentication needed.
  Tools: search_model (find CDS definitions), search_docs (CAP documentation)
  Tip: Search CDS definitions before reading .cds files; check CAP docs when making model changes.
```

---

## Phase 15: GitHub MCP

**Skip if already installed.**

Source: [github/github-mcp-server](https://github.com/github/github-mcp-server)

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► GITHUB (15/17)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

GitHub MCP enables repository management, issues, PRs, and Actions on github.com.
Provides 100+ tools for GitHub integration.
Requires a GitHub Personal Access Token (PAT).
```

### Step 15.1: Confirm installation

Use AskUserQuestion:

- header: "Install GitHub?"
- question: "GitHub MCP provides repository, issue, PR, and Actions management for github.com. Requires a Personal Access Token. Continue?"
- options:
  - label: "Yes, install"
    description: "Add GitHub MCP to Claude Code"
  - label: "Skip"
    description: "Skip GitHub MCP installation"

**If user selects "Skip":** Skip to Phase 16 (SAP GitHub Tools).

### Step 15.2: Gather credentials

Use AskUserQuestion to collect:

**Question 1 - GitHub PAT:**
- header: "GitHub PAT"
- question: "Enter your GitHub Personal Access Token. Create one at https://github.com/settings/personal-access-tokens/new with repo, issues, and pull_request scopes."
- Allow free text input

### Step 15.3: Add to Claude

```bash
claude mcp add github -s user \
  -e GITHUB_PERSONAL_ACCESS_TOKEN="{github_pat}" \
  -- npx -y @modelcontextprotocol/server-github@latest
```

If this fails, suggest checking:
- Node.js is installed (`node --version`)
- Token has correct scopes

### Step 15.4: Verify

Verify by reading `~/.claude.json` and checking for the `github` key in `mcpServers`.

Display:
```
✓ GitHub MCP installed
  Note: Uses Personal Access Token for authentication.
  Tools: 100+ tools for repos, issues, PRs, Actions, code security, and more
```

---

## Phase 16: SAP GitHub Tools MCP

**Skip if already installed.**

Source: [github/tools-github-mcp](https://github.tools.sap/github/tools-github-mcp)

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► SAP GITHUB TOOLS (16/17)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SAP GitHub Tools MCP enables repository management on SAP's internal GitHub (github.tools.sap).
Requires a Personal Access Token from github.tools.sap.
```

### Step 16.1: Confirm installation

Use AskUserQuestion:

- header: "Install SAP GitHub Tools?"
- question: "SAP GitHub Tools MCP provides integration with github.tools.sap. Requires a PAT from github.tools.sap. Continue?"
- options:
  - label: "Yes, install"
    description: "Add SAP GitHub Tools MCP to Claude Code"
  - label: "Skip"
    description: "Skip SAP GitHub Tools MCP installation"

**If user selects "Skip":** Skip to Phase 17 (SAP WDF GitHub).

### Step 16.2: Gather credentials

Use AskUserQuestion to collect:

**Question 1 - SAP GitHub Tools PAT:**
- header: "SAP GitHub PAT"
- question: "Enter your github.tools.sap Personal Access Token. Create one at https://github.tools.sap/settings/tokens with repo scope."
- Allow free text input

### Step 16.3: Add to Claude

```bash
claude mcp add github-tools-sap -s user \
  -e GITHUB_PERSONAL_ACCESS_TOKEN="{sap_github_pat}" \
  -- npx -y git+https://github.tools.sap/github/tools-github-mcp.git
```

If this fails, suggest checking:
- Node.js version is 20+ (`node --version`)
- Access to github.tools.sap (VPN/network)
- Token has correct scopes

### Step 16.4: Verify

Verify by reading `~/.claude.json` and checking for the `github-tools-sap` key in `mcpServers`.

Display:
```
✓ SAP GitHub Tools MCP installed
  Note: Uses Personal Access Token for github.tools.sap authentication.
  Tools: Repository, issues, PRs, and Actions management for SAP internal GitHub
```

---

## Phase 17: SAP WDF GitHub MCP

**Skip if already installed.**

Source: [sap.tools.hyperspace/github-wdf-mcp-server](https://github.tools.sap/sap.tools.hyperspace/github-wdf-mcp-server)

Display banner:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► SAP WDF GITHUB (17/17)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SAP WDF GitHub MCP enables repository management on SAP's WDF GitHub instance.
Requires a Personal Access Token from the WDF GitHub instance.
```

### Step 17.1: Confirm installation

Use AskUserQuestion:

- header: "Install SAP WDF GitHub?"
- question: "SAP WDF GitHub MCP provides integration with SAP's WDF GitHub instance. Requires a PAT. Continue?"
- options:
  - label: "Yes, install"
    description: "Add SAP WDF GitHub MCP to Claude Code"
  - label: "Skip"
    description: "Skip SAP WDF GitHub MCP installation"

**If user selects "Skip":** Skip to Phase 18 (Summary).

### Step 17.2: Gather credentials

Use AskUserQuestion to collect:

**Question 1 - SAP WDF GitHub PAT:**
- header: "WDF GitHub PAT"
- question: "Enter your SAP WDF GitHub Personal Access Token with repo scope."
- Allow free text input

### Step 17.3: Add to Claude

```bash
claude mcp add github-wdf -s user \
  -e GITHUB_PERSONAL_ACCESS_TOKEN="{wdf_github_pat}" \
  -- npx -y git+https://github.tools.sap/sap.tools.hyperspace/github-wdf-mcp-server.git
```

If this fails, suggest checking:
- Node.js version is 20+ (`node --version`)
- Access to github.tools.sap (VPN/network)
- Token has correct scopes

### Step 17.4: Verify

Verify by reading `~/.claude.json` and checking for the `github-wdf` key in `mcpServers`.

Display:
```
✓ SAP WDF GitHub MCP installed
  Note: Uses Personal Access Token for SAP WDF GitHub authentication.
  Tools: Repository, issues, PRs, and Actions management for SAP WDF GitHub
```

---

## Phase 18: Summary

Display final status:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 INSTALL MCPs ► COMPLETE ✓
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Installation summary:

| MCP | Status | Auth Method |
|-----|--------|-------------|
| Databricks | ✓ | OAuth (browser) |
| Productboard | ✓ | Hosted (VPN) |
| Atlassian | ✓ | OAuth (browser) |
| Microsoft 365 | ✓ | Device code |
| SAP Auth | ✓ | Browser SSO |
| SAP Wiki | ✓ | Cookie (via SAP Auth) |
| SAP KBA | ✓ | Browser (Selenium) |
| SAP Jira | ✓ | Cookie (via SAP Auth) |
| SAP Help | ✓ | None (open APIs) |
| Playwright | ✓ | None |
| Figma | ✓ | OAuth (browser) |
| SAP Fiori | ✓ | None |
| Context7 | ✓ | None |
| CAP | ✓ | None |
| GitHub | ✓ | Personal Access Token |
| SAP GitHub Tools | ✓ | Personal Access Token |
| SAP WDF GitHub | ✓ | Personal Access Token |

**Next steps:**
1. **Restart Claude Code** to load the new MCPs
2. Run `claude mcp list` to verify connections (works from terminal, not from within Claude Code)
3. For Databricks/Atlassian/Figma: sign in via browser on first use
4. For SAP Auth/Wiki/Jira: authentication triggers automatically via browser on first use
5. For SAP KBA: uses built-in browser auth on first use (Chrome/Chromium required)
6. SAP Help, Playwright, SAP Fiori, Context7, and CAP work immediately (no auth needed)
7. For GitHub MCPs: ensure your Personal Access Tokens have correct scopes

**Troubleshooting:**
- If an MCP shows disconnected, check credentials
- Run `/install-mcps` again to reinstall any failed MCPs
- For Databricks OAuth issues, delete ~/.databricks/token-cache.json
- For SAP Auth issues, check Node.js 20+ and browser availability
- SAP Auth logs: `~/.sap-mcp/logs/sap-auth-mcp.log` (set VERBOSE=true to enable)
- For SAP Wiki/Jira issues, ensure SAP Auth MCP is installed and working first
- For SAP KBA issues, check Python 3.10+, uv, Chrome/Chromium, and repo access
- For SAP Help/Fiori/CAP issues, check Node.js 20+ and npx
- For Playwright issues, check Node.js and npx are working
- For Figma issues, check network access to mcp.figma.com
- For GitHub MCP issues, verify PAT scopes (repo, issues, pull_request)
- For SAP GitHub MCPs, ensure VPN connection and access to github.tools.sap
- To update cloned MCPs: `cd ~/.local/share/mcp-servers/<name> && git pull && uv sync`
```

---

## Error Handling

**If uv tool install fails:**
- Display the error message
- Suggest: "Check that the MCP directory exists and contains a valid pyproject.toml"
- Ask if user wants to continue with remaining MCPs

**If claude mcp add fails:**
- Display the error message
- Offer to retry with new credentials via AskUserQuestion

**If already exists warning:**
- Run `claude mcp remove <name> -s user` first, then re-add with the new configuration
- This applies to all MCPs, not just Productboard
