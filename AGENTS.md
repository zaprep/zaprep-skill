# Zaprep (for Codex, Cursor and other agents that read AGENTS.md)

When the user asks about Instagram or Facebook automations (comment-to-DM, DM keyword replies, story replies, Follow Gate, email capture), their Zaprep inbox, or Instagram/Facebook insights, use the Zaprep MCP server and follow [skills/zaprep/SKILL.md](skills/zaprep/SKILL.md). Ready-made automations are in [skills/zaprep/references/recipes.md](skills/zaprep/references/recipes.md).

Connect the server once:

- Codex: `codex mcp add zaprep --url https://www.zaprep.com/api/mcp`, then `codex mcp login zaprep`
- Cursor and others: add a remote MCP server with URL `https://www.zaprep.com/api/mcp` and sign in with your Zaprep account

The most important rules, in case you read nothing else:

- New automations are saved paused. Show the user what will be sent and turn it on with `toggle_automation` only after they confirm.
- Keywords go in `triggerConfig.keywords` as an array, e.g. `{"keywords": ["guide"]}`. No keywords means every comment triggers the rule.
- Message variables use double braces: `{{first_name}}`, `{{username}}`, `{{mention}}`, `{{account_username}}`.
- Get every id from the tools (`list_accounts`, `list_posts`, `resolve_post`, `list_automations`); never guess one.
