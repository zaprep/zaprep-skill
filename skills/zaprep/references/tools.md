# Zaprep MCP tools

54 tools. "changes" = writes to your Zaprep workspace; everything else only reads. Live list: https://www.zaprep.com/mcp/tools

## Automations

Comment-to-DM, DM keyword, story and follower automations. New ones are saved paused for review.

- `list_automations`: List automation rules.
- `create_automation` (changes): Create a new automation rule.
- `update_automation` (changes): Edit an existing automation rule.
- `toggle_automation` (changes): Enable or disable an automation rule
- `duplicate_automation` (changes): Copy an automation rule.
- `delete_automation` (changes): Permanently delete an automation rule

## Scheduled automations

Arm an automation for the next post an account publishes.

- `list_scheduled_automations`: List comment automations pre-armed for the next post an Instagram account publishes (bound to that post automatically once it goes live).
- `create_scheduled_automation` (changes): Arm a comment automation for the next post this Instagram account publishes.
- `update_scheduled_automation` (changes): Replace a still-waiting scheduled automation (send the full body).
- `delete_scheduled_automation` (changes): Cancel a still-waiting scheduled automation.

## Accounts, posts and comments

Find the account and the post an automation should run on.

- `list_accounts`: List connected Instagram accounts and Facebook Pages.
- `list_posts`: List an Instagram account's posts and reels, or a Facebook Page's posts, newest first.
- `resolve_post`: Turn a post link into the post object, including its id for triggerConfig.post_id.
- `list_post_comments`: List the public comments (with replies) on a post.
- `list_stories`: List an Instagram account's live stories (posted in the last 24 hours): id, type and when each was posted.
- `sync_posts` (changes): Refresh Zaprep's copy of an account's posts from Instagram or Facebook (new posts, captions and engagement numbers).

## Insights and post performance

Reach, views and follower growth, and which posts and reels perform best.

- `get_account_insights`: Instagram account insights for the last 7, 30 or 90 days: totals and a daily series of reach, views, profile views, accounts engaged, interactions and net follower change, plus audience (age, gender, countries, cities) when the account has 100+ followers.
- `get_post_performance`: Rank an account's posts and reels (Instagram) or Page posts (Facebook) by a metric, from Zaprep's synced copy.

## Inbox

Read your DM conversations. Replies are still sent from the Zaprep inbox.

- `list_conversations`: List recent inbox conversations (Instagram DMs, Facebook Page messages), newest first.
- `get_conversation_messages`: Read the messages of one inbox conversation (id from list_conversations), oldest first, 50 or so per page.

## Results and troubleshooting

How automations perform, and why one is not firing.

- `get_analytics`: Dashboard analytics: messages sent (DMs / comment replies), per-day series, followers gained, automations, 30-day views, link clicks.
- `get_automation_metrics`: Per-rule stats keyed by rule id: runs, DMs sent, followers gained, link clicks, and sends blocked by the 24h window or the monthly limit.
- `get_automation_funnel`: Per automation: times triggered, DMs sent, contacts collected (all time), most-triggered first.
- `get_automation_activity`: Recent activity for one automation: messages sent, senders waiting in the Follow Gate queue, and every sender it has fired for.
- `troubleshoot_automation`: Diagnose why an automation may not be firing: paused rule, disconnected account, invalid/expiring token, webhook problems, retrigger limits, Follow Gate queue.
- `get_usage`: Current plan and usage vs. limits: messages (contacts) this month, connected accounts, automations.

## AI suggestions and AI Replies

Draft automations from captions, and set up AI that answers DMs.

- `suggest_automation`: AI-draft a full automation (name, trigger, keywords, comment reply, DM sequence) from a post caption.
- `analyze_post`: AI-analyze a post caption for automation potential (category, intent, score, suggested CTA and keywords).
- `bulk_suggest_automations`: AI-draft automations for up to 20 recent cached posts of an account that have a caption and no automation yet (capped by the remaining monthly AI quota).
- `get_auto_dm`: Get the Auto AI Reply (Auto DM) setup for an Instagram account or Facebook Page: on/off, fallback messages, idle wait, cooling, audience, and the AI persona instructions.
- `update_auto_dm` (changes): Create or edit the Auto AI Reply for an Instagram account or Facebook Page (one per account).
- `get_ai_avatar`: Get an Instagram account's or Facebook Page's AI persona (business type, tone, important instruction, about, audience) and the compiled instructions the AI replies with.
- `update_ai_avatar` (changes): Set an Instagram account's or Facebook Page's AI persona used by Auto AI Reply.

## Contacts

Everyone who commented, messaged or shared an email.

- `list_contacts`: List collected contacts (commenters/DMers and emails collected by automations), across all accounts.
- `export_contacts_csv`: Export collected contacts as CSV text (Username, IG User ID, Email, Follower, Collected At).

## Links, Link in Bio and media kit

Trackable short links, editable QR codes and your public pages.

- `list_links`: List the workspace's short links (zaprep.com/l/<code>) with click counts.
- `create_link` (changes): Create a trackable short link for an http(s) URL.
- `update_link` (changes): Change where a short link redirects (by id from list_links).
- `delete_link` (changes): Delete a short link by id (from list_links).
- `list_qr_codes`: List the workspace's QR codes: name, the short URL each code encodes, where it currently redirects, colors and scan count.
- `create_qr_code` (changes): Create a QR code that opens `url`.
- `update_qr_code` (changes): Rename a QR code, change where it redirects, or change its colors (by id from list_qr_codes).
- `delete_qr_code` (changes): Delete a QR code by id.
- `get_link_in_bio`: Get the workspace's Link in Bio pages (zaprep.com/p/<username>): the page for an Instagram account (or the first one), its links, the list of all pages, and suggested defaults when none exists yet.
- `update_link_in_bio` (changes): Create or edit a Link in Bio page.
- `add_link_in_bio_link` (changes): Add a link block (button) to a Link in Bio page.
- `update_link_in_bio_link` (changes): Edit a Link in Bio link block (title, url, description, emoji, isActive, sortOrder).
- `reorder_link_in_bio_links` (changes): Reorder a page's link blocks: pass the full ordered list of link ids.
- `delete_link_in_bio_link` (changes): Delete a Link in Bio link block.
- `get_media_kit`: Get the media kit (zaprep.com/m/<slug>) for an Instagram account, or the first account if omitted.
- `update_media_kit` (changes): Create or edit an Instagram account's media kit (rate card, collabs, demographics, stat overrides, publish).

## Profile and referrals

Your Zaprep profile and Refer & Earn balance.

- `get_profile`: Get the account owner's Settings → Profile (name, title, bio, phone, social URLs).
- `update_profile` (changes): Edit the account owner's Settings → Profile.
- `get_referral_info`: Refer & Earn: referral code and link, earnings (today/7d/lifetime), unpaid balance, referred users and reward history.
