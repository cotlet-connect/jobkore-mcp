# Jobkore API and MCP server for ChatGPT and Claude

Jobkore publishes no public API or MCP server (checked 17 November 2026 on the linked integration page). Cotlet.dev connects ChatGPT, Claude and other AI agents to your own Jobkore account through a hosted MCP server with 20 tools for estimates, invoices, change orders, payments, field notes and subcontractor balances.

## What you can ask

> Show what clients still owe and which invoices are past due.

> Turn selected field notes into an estimate draft with Hank AI.

> Create an invoice from an accepted estimate.

> Add a change order to a sent invoice for signature.

> Record a check payment and show the remaining balance.

> Show what we owe each subcontractor on a job.

## Connect your account

Request access at https://cotlet.dev/signup?app=jobkore and sign in to Jobkore in your Cotlet Cloud Browser. For editors and scripts, get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its Jobkore tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run `python3 jobkore_agent.py serve` from this repository, or use Docker below.

Or with Docker:

```bash
docker build -t jobkore-mcp .
docker run -i --rm -e COTLET_API_KEY jobkore-mcp
```

Tools: `jobkore_guide`, `jobkore_status`, `jobkore_open`, `jobkore_read`, `jobkore_click`, `jobkore_type`, `jobkore_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From this repository:

```bash
export COTLET_API_KEY="your-key"
python3 jobkore_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 jobkore_agent.py status    # is Jobkore ready in your Cotlet.dev account
python3 jobkore_agent.py guide     # Jobkore screens and sign-in link
python3 jobkore_agent.py read      # open Jobkore, read the page, close the browser
python3 jobkore_agent.py tools     # list Cotlet.dev tools
python3 jobkore_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Jobkore users

**Does Jobkore have a public API or MCP server?**

The linked integration page reports no public Jobkore API, MCP server or official ChatGPT or Claude connector as of 17 November 2026.

**What can I do with Jobkore through Cotlet.dev?**

The hosted integration lists 20 tools for contractor office work, including estimates, invoices, change orders, payment records, subs, leads and field notes. This local adapter exposes 7 browser helper tools and forwards their calls to Cotlet.dev.

**How do I connect my account?**

Request access at https://cotlet.dev/signup?app=jobkore, sign in to app.getjobkore.com in your Cotlet Cloud Browser, then add https://cotlet.dev/mcp/v1 to your agent. Chat apps use Cotlet sign-in; editors and scripts use a console API key.

**Does the agent send documents without asking?**

Reads run immediately. Writes ask for confirmation by default; you can change the approval setting for each tool.

**Is this an official Jobkore integration?**

No. Cotlet.dev is independent and is not affiliated with, endorsed by or sponsored by Jobkore.

## Source

https://cotlet.dev/integrations/jobkore-api-mcp-server

---

More: https://cotlet.dev/integrations/jobkore-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=jobkore

Request access: https://cotlet.dev/signup?app=jobkore&utm_source=github&utm_medium=readme&utm_campaign=jobkore
