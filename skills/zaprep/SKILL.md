---
name: zaprep
description: Set up and manage Instagram and Facebook Page automations with Zaprep (comment-to-DM, DM keyword replies, story replies, Follow Gate, email capture, AI Replies), read the DM inbox, and report on insights, top posts and automation results. Use when the user mentions Zaprep, or asks to auto-reply to comments or DMs, send a link when someone comments a keyword, grow followers with a follow-to-unlock DM, collect emails from Instagram, audit or troubleshoot automations, or report on Instagram or Facebook performance.
---

# Zaprep

Zaprep automates Instagram and Facebook Page conversations: when someone comments a keyword, messages the account, replies to a story or follows, Zaprep replies publicly and/or sends a DM. You work with it through the Zaprep MCP server (`https://www.zaprep.com/api/mcp`, 54 tools). Full tool list: [references/tools.md](references/tools.md). Ready-made setups: [references/recipes.md](references/recipes.md).

If no `zaprep` tools are available, tell the user to connect the server first: in Claude Code the plugin adds it (run `/mcp` to sign in), in claude.ai go to Settings → Connectors → Add custom connector → `https://www.zaprep.com/api/mcp`. Sign-in is OAuth with their Zaprep account; a free account works.

## Safety rules

1. **New automations are saved paused.** `create_automation` defaults to `activate: false`. Show the user exactly what will be sent (trigger, keywords, public reply, DM, buttons) and only call `toggle_automation` with `isActive: true` after they confirm. Pass `activate: true` only if they explicitly asked to go live immediately.
2. **Ask before anything destructive or public-facing**: deleting, pausing a live automation, editing a live automation's messages, or turning one on.
3. **Never invent ids.** Get `accountId` from `list_accounts`, post ids from `list_posts` / `resolve_post`, rule ids from `list_automations`.
4. **Sending DMs or comment replies by hand is not possible over MCP.** Inbox tools are read-only; point the user to the Zaprep inbox for manual replies.
5. Use real numbers from the tools only. Never estimate results.

## Workflow: comment-to-DM on a post

1. `list_accounts` → pick the account. If `needsReconnect` is true, stop and tell the user to reconnect at https://www.zaprep.com/accounts (nothing will send until they do).
2. Find the post: if the user gives a link, `resolve_post` (or pass `triggerConfig.post_url` directly). Otherwise `list_posts`. For a brand-new post, `sync_posts` first.
3. `create_automation`:
   ```json
   {
     "accountId": "<from list_accounts>",
     "name": "GUIDE comment -> guide DM",
     "triggerType": "comment",
     "triggerConfig": { "post_id": "<id>", "keywords": ["guide"] },
     "actions": [
       { "type": "reply_comment", "messages": ["Sent! Check your DMs {{first_name}}", "Just sent it over!", "Check your inbox 📩"] },
       { "type": "send_dm", "message": "Hey {{first_name}}! Here's the guide:", "buttons": [{ "title": "Get the guide", "url": "https://example.com/guide" }] }
     ]
   }
   ```
4. Show it to the user, then activate with `toggle_automation` after they confirm.

## How triggers and keywords work

- `triggerType`: `comment`, `dm`, `story_reply`, `story_reaction`, `new_follower`, `reel_share`. **Facebook Pages support only `comment` and `dm`.**
- **Keywords always go in `triggerConfig.keywords` as an array**, e.g. `["price", "cost"]`. Matching ignores case. `match_mode`: `any` (default, the comment contains any keyword), `all` (contains every keyword), `exact` (the whole comment equals a keyword).
- **No keywords means every comment (or DM) triggers it.** Only do that when the user asks for it.
- `post_id` scopes a comment rule to one post. A comment rule without `post_id` is a catch-all that runs on every post that does not have its own post-scoped rule.
- `story_reaction`, `new_follower` and `reel_share` have no text, so they take no keywords.
- In practice Instagram delivers very few `new_follower` and `story_reaction` events, so don't promise users these will fire. `comment`, `dm`, `story_reply` and `reel_share` are reliable.
- Retrigger: by default a person triggers a rule once. `retriggerEnabled` with `retriggerValue` + `retriggerUnit` (`seconds` or `minutes`, minimum 30) lets them trigger it again after a cooldown.

## Actions

| type | What it does | Limits |
|---|---|---|
| `reply_comment` | Public reply under the comment | 150 characters per variation, up to 4 `messages` (one picked at random) |
| `send_dm` | Private DM | 1000 characters, up to 3 `buttons` `{title, url}`, optional `image_url` (then text max 80 characters) |
| `ask_email` | Asks for an email in DM and saves it to contacts | Paid plans for new automations |
| `like_comment` | Likes the triggering comment | Works on Facebook; Instagram may refuse |

Message variables use **double braces**: `{{first_name}}`, `{{username}}`, `{{mention}}` (tags the commenter in a Facebook public reply), `{{account_username}}`. Single braces like `{username}` are sent as literal text.

Give `reply_comment` 2 to 4 different variations so replies don't look like spam.

## Instagram and Meta rules that explain most "it didn't send" questions

- **One private DM per comment.** The first DM to a commenter answers their comment, and Meta allows exactly one. Follow-up messages only arrive if the person has messaged the account in the last 24 hours.
- A comment can be answered by DM for 7 days.
- DMs to people who never messaged the account may land in **Message Requests**, not the main inbox.
- Automations only react to comments made **after** they are turned on.
- If Instagram restricts the account (Account Status in the Instagram app), every automated DM fails until the restriction ends. No tool can get around it.

For any automation that fires but does not send, call `troubleshoot_automation` and explain the cause in plain words.

## Follow Gate (Instagram only, paid plans)

Set `triggerConfig.require_follow: true` to send the main DM only to followers. Non-followers get a "follow us, then tap I'm following" message first. Options: `gate_skip_dm_for_followers`, `gate_show_to_followers`. Not available on Facebook Pages or the Free plan.

## WhatsApp (beta)

WhatsApp numbers appear in `list_accounts` with `platform: "whatsapp"` only for workspaces in the beta. If the user asks about WhatsApp and no such account is listed, tell them it is in beta and Zaprep support can turn it on.

- Only the `dm` trigger, with `send_dm` and `ask_email` actions. No posts, comments, story triggers or Follow Gate.
- Use at most one link button per DM: WhatsApp shows one link button; extra links are written into the message text.
- Zaprep only replies to messages people send to the number. It cannot start conversations or send broadcasts.
- Needs a paid plan.
- Inbox tools (`list_conversations`, `get_conversation_messages`) include WhatsApp chats.

```json
{
  "accountId": "<whatsapp account from list_accounts>",
  "name": "MENU on WhatsApp",
  "triggerType": "dm",
  "triggerConfig": { "keywords": ["menu"] },
  "actions": [{ "type": "send_dm", "message": "Hi {{first_name}}! Here's our menu:", "buttons": [{ "title": "See menu", "url": "https://example.com/menu" }] }]
}
```

## Reporting

- Account growth: `get_account_insights` (Instagram, `days` 7, 30 or 90). If `insightsAvailable` is false, the account needs reconnecting to grant insights.
- Best content: `get_post_performance` (`sortBy` views, reach, interactions, likes, comments, shares or saves; `days` to limit the window).
- Automation results: `get_analytics`, `get_automation_metrics`, `get_automation_funnel`, `get_automation_activity`.
- Inbox: `list_conversations` (`unreadOnly: true` for people waiting on a reply), then `get_conversation_messages`.
- Plan and usage: `get_usage`.

Keep reports short: real numbers, what changed, and 2 or 3 concrete next steps.

## Other things you can manage

AI Replies (`get_auto_dm`, `update_auto_dm`, `get_ai_avatar`, `update_ai_avatar`), scheduled automations for the next post (`create_scheduled_automation`), AI-drafted automations from captions (`suggest_automation`, `bulk_suggest_automations`), contacts (`list_contacts`, `export_contacts_csv`), short links and QR codes, Link in Bio pages and the media kit. See [references/tools.md](references/tools.md).

Not available over MCP (send users to https://www.zaprep.com): billing and plans, connecting accounts, team members, API keys.
