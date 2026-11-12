# FantasyPros AI plugin

Fantasy football advice from FantasyPros, inside your AI assistant. The plugin
connects your assistant to the **FantasyPros MCP server**, which serves expert
consensus rankings, projections, injury reports, depth charts, and your synced
leagues. It also adds **skills**, step-by-step workflows that tell the
assistant which tools to call for common in-season questions.

It works with Claude (Claude Code, the Claude apps, and claude.ai), Gemini CLI,
and OpenAI Codex. ChatGPT and any other MCP client can connect to the server
directly.

## What you need

- A FantasyPros account. You sign in with it the first time the assistant uses
  the server.
- To get advice about your own team, a league synced at
  [fantasypros.com/sync](https://www.fantasypros.com/sync/).
- Some tools depend on your FantasyPros plan. See
  [What tools are available in the FantasyPros MCP Server](https://support.fantasypros.com/hc/en-us/articles/55238312588571-What-tools-are-available-in-the-FantasyPros-MCP-Server).

## Install

### Claude Code

```
/plugin marketplace add fantasypros/fantasypros-ai-plugin
/plugin install fantasypros@fantasypros-ai-plugin
```

Run `/mcp`, choose **fantasypros**, and sign in with your FantasyPros account.

### Claude apps and claude.ai

Go to **Customize → Plugins**, add the marketplace
`fantasypros/fantasypros-ai-plugin`, and install **FantasyPros**. Then open the
plugin's **Connectors** tab and connect FantasyPros.

### Gemini CLI

```
gemini extensions install https://github.com/fantasypros/fantasypros-ai-plugin
```

Start Gemini CLI and run `/mcp auth fantasypros` to sign in.

### OpenAI Codex

```
codex plugin marketplace add fantasypros/fantasypros-ai-plugin
```

Run `/plugins`, install **FantasyPros**, and sign in when prompted.

### ChatGPT and other MCP clients

Add a remote MCP server with this URL:

```
https://api.fantasypros.com/mcp
```

The server uses OAuth, so your client prompts you to sign in with your
FantasyPros account. This path gives you the tools but not the skills below.

## Skills

| Skill | Use it for |
| --- | --- |
| `start-sit` | Weekly lineup calls: who to start, X or Y, setting your whole lineup |
| `waiver-wire` | Pickups, who to drop, and whether a specific add/drop is worth it |
| `trade-finder` | Trade ideas and targets that work for both sides |
| `player-compare` | Comparing two to four players for this week or the rest of the season |
| `injury-check` | A player's injury status, practice report, and latest news |

Your assistant picks the right skill from your question. Try:

- "Who should I start at FLEX this week?"
- "Best waiver pickups for my team this week"
- "Find me a trade to upgrade at RB"
- "Is my running back playing this week?"

## How answers use FantasyPros data

The skills tell the assistant to use FantasyPros tools for rankings,
projections, start/sit advice, injuries, depth charts, schedules, and your
league, and not to override them with web results. For anything the tools
don't cover, such as general news, the assistant may search the web. It
prefers recent sources and says where the information came from.

## About this repository

FantasyPros maintains this plugin in an internal repository, and each update
there is copied here. Pull requests can't be merged directly, but please
[open an issue](https://github.com/fantasypros/fantasypros-ai-plugin/issues)
for bugs or suggestions.

- [Privacy Policy](https://www.fantasypros.com/about/privacy/)
- [Terms of Use](https://www.fantasypros.com/about/legal/)
- [Contact FantasyPros](https://www.fantasypros.com/about/contact/)
