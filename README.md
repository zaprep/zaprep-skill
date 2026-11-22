# Zaprep skill for Claude

Let Claude set up and run your Instagram and Facebook automations with [Zaprep](https://www.zaprep.com): comment-to-DM, DM keyword replies, story replies, Follow Gate, email capture and AI Replies. Claude can also read your DM inbox and report on insights, top posts and automation results.

This repo contains:

- **The Zaprep skill** (`skills/zaprep/SKILL.md`): how to use Zaprep's tools well, the safety rules (new automations are saved paused until you confirm), how keywords and triggers work, and the Instagram/Meta rules behind most "it didn't send" questions.
- **The Zaprep MCP server** config (`.mcp.json`): `https://www.zaprep.com/api/mcp`, 54 tools.
- **Recipes** (`skills/zaprep/references/recipes.md`): ready-made automations.

You need a Zaprep account (the Free plan works) with an Instagram professional account or Facebook Page connected.

## Install

### Claude Code

```
/plugin marketplace add zaprep/zaprep-skill
/plugin install zaprep@zaprep
```

Then run `/mcp`, pick `zaprep` and sign in with your Zaprep account.

### Claude.ai and Claude Desktop

1. Add the connector: Settings → Connectors → Add custom connector → `https://www.zaprep.com/api/mcp`, then sign in.
2. Add the skill: download this repo as a ZIP, zip the `skills/zaprep` folder on its own, and upload it in Settings → Capabilities → Skills.

### Gemini CLI

```
gemini extensions install https://github.com/zaprep/zaprep-skill
```

This adds the Zaprep MCP server and loads the skill as context. Then sign in to the `zaprep` server with your Zaprep account (in Gemini CLI, run `/mcp` to see its status).

### Codex

```
codex mcp add zaprep --url https://www.zaprep.com/api/mcp
codex mcp login zaprep
```

Copy `AGENTS.md` (and the `skills` folder) into your project, or paste the skill into your instructions.

### ChatGPT, Cursor and other AI tools

Connect `https://www.zaprep.com/api/mcp` as a remote MCP server (setup guides at [zaprep.com/mcp](https://www.zaprep.com/mcp)). The server sends its own usage rules to every client, so it works without the skill; if your tool supports skills or custom instructions, add `skills/zaprep/SKILL.md` for the full guide.

## Try it

- "Set up comment-to-DM on my latest reel: when someone comments GUIDE, DM them https://example.com/guide."
- "Audit my Zaprep automations and tell me what to fix."
- "Which of my reels from the last 30 days got the most views?"
- "Who is waiting on a reply in my inbox?"
- "Write my weekly Instagram report."

## Safety

- Automations Claude creates are saved **paused**. Claude shows you what will be sent and turns them on only after you confirm.
- Inbox access is read-only: Claude can't send DMs or comment replies by hand.
- Billing, connecting accounts, team members and API keys stay in the Zaprep dashboard.
- You can revoke access any time at [zaprep.com/settings/claude](https://www.zaprep.com/settings/claude).

## Links

- Tools reference: [zaprep.com/mcp/tools](https://www.zaprep.com/mcp/tools)
- Developer docs: [zaprep.com/mcp/docs](https://www.zaprep.com/mcp/docs)
- Privacy policy: [zaprep.com/privacy](https://www.zaprep.com/privacy)
- Support: support@zaprep.com

## License

MIT
