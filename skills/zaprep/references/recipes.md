# Zaprep recipes

Ready-made automations. Replace the ids, keywords and links. Every one is created paused: show it to the user, then turn it on with `toggle_automation` after they confirm.

## 1. Lead magnet: comment a keyword, get a link by DM

```json
{
  "triggerType": "comment",
  "triggerConfig": { "post_id": "<post id>", "keywords": ["guide"] },
  "actions": [
    { "type": "reply_comment", "messages": ["Sent! Check your DMs 📩", "Just sent it {{first_name}}!", "On its way to your inbox"] },
    { "type": "send_dm", "message": "Hey {{first_name}}! Here's your free guide 👇", "buttons": [{ "title": "Get the guide", "url": "https://example.com/guide" }] }
  ]
}
```

## 2. Product link on every post (catch-all)

No `post_id`: runs on every post that has no post-specific automation.

```json
{
  "triggerType": "comment",
  "triggerConfig": { "keywords": ["price", "link", "shop"] },
  "actions": [
    { "type": "reply_comment", "messages": ["Sent you the details!", "Check your DMs {{first_name}}"] },
    { "type": "send_dm", "message": "Here you go {{first_name}}:", "buttons": [{ "title": "Shop now", "url": "https://example.com/shop" }] }
  ]
}
```

## 3. Follow to unlock (Instagram, paid plans)

```json
{
  "triggerType": "comment",
  "triggerConfig": { "post_id": "<post id>", "keywords": ["free"], "require_follow": true },
  "actions": [
    { "type": "reply_comment", "messages": ["Check your DMs!"] },
    { "type": "send_dm", "message": "Thanks for following {{first_name}}! Here it is:", "buttons": [{ "title": "Open", "url": "https://example.com/free" }] }
  ]
}
```

## 4. Collect emails (paid plans)

```json
{
  "triggerType": "comment",
  "triggerConfig": { "post_id": "<post id>", "keywords": ["waitlist"] },
  "actions": [
    { "type": "reply_comment", "messages": ["Sent you a DM!"] },
    { "type": "ask_email", "prompt": "Hey {{first_name}}! What's the best email to add you to the waitlist?" }
  ]
}
```

Collected emails: `list_contacts` or `export_contacts_csv`.

## 5. DM keyword auto-reply

```json
{
  "triggerType": "dm",
  "triggerConfig": { "keywords": ["menu"] },
  "actions": [
    { "type": "send_dm", "message": "Here's our menu {{first_name}} 👇", "buttons": [{ "title": "See menu", "url": "https://example.com/menu" }] }
  ]
}
```

## 6. Story replies (Instagram)

```json
{
  "triggerType": "story_reply",
  "triggerConfig": { "keywords": ["yes"] },
  "actions": [
    { "type": "send_dm", "message": "Love it {{first_name}}! Here's the link:", "buttons": [{ "title": "Open", "url": "https://example.com" }] }
  ]
}
```

## Prompts that work well

- "Audit my Zaprep automations and tell me what to fix."
- "Which of my reels from the last 30 days got the most views, and which have no automation?"
- "Set up comment-to-DM on my latest reel: keyword GUIDE, send https://example.com/guide."
- "Who is waiting on a reply in my inbox?"
- "Write my weekly Instagram report."
