# Telegram Agent Gateway

Let your AI agent read **only the Telegram chats you pick**, from a small server that belongs to you.

- You put chats in a Telegram folder called `Agent`. Your agent can read those, and nothing else.
- Add a chat to the folder to share it. Remove it to take it away. It updates within seconds.
- Your agent gets a web address and a token. It never gets your Telegram login.
- It only reads. It can't send, edit or delete anything.
- **It never marks messages as read**, never marks voice messages as listened, and never shows
  you as online. People in the chat can't tell your agent looked.
- It sees what you'd see: text, hidden links, screenshots and files, reactions, pinned messages,
  polls, edits, link previews, forum topics, and voice messages as text (Telegram's own
  transcription, if you have Premium).

> **Using an AI agent to help you set this up?** Give it this page and say *"walk me through
> this"*. There's a section at the bottom written for it.

---

## What you need

- **A Telegram account.** Premium is optional; it only adds voice-message transcripts.
- **A GitHub account.** Free. Make one at [github.com/signup](https://github.com/signup) if you don't have it.
- **About $7.25 a month** for the server on Render (a card is needed).
- **About 15 minutes.**

---

## Set it up

### Step 1: Copy this project to your own GitHub ("fork")

1. Make sure you're logged in to GitHub.
2. At the top right of this project's page, click **Fork**, then **Create fork**.

Or let your agent do it in one command (Claude Code and others that use GitHub's `gh` tool):

```bash
gh repo fork ZD0101/telegram-agent-gateway --clone=false
```

You now have your own copy, and **everything from here uses your copy, on your own GitHub
account**. Your server runs from it, so nobody else can change what runs on your server. Your
fork is public like this project, and that's fine: the code holds no secrets. Your Telegram
keys and login only ever go to your own server.

### Step 2: Put it on a server (Render)

1. Go to [dashboard.render.com](https://dashboard.render.com) and click **GitHub** to sign in.
   That creates your Render account at the same time.
2. GitHub asks what Render may see. Choose **Only select repositories**, pick **your**
   `telegram-agent-gateway` (the one under your name), and confirm.
3. In Render, click **+ New** (top right) → **Blueprint**.
   Not "Web Service". Blueprint reads the settings from the project for you, including the disk
   that keeps your login.
4. Pick **telegram-agent-gateway**.
   If the **Blueprint Path** box is empty, click it and choose `render.yaml`.
   Name it anything.
5. You'll see one service on the **Starter** plan with a 1 GB disk. Click **Deploy Blueprint**
   and add your card when asked.
6. Wait until it says **Live**. The first time takes 3–5 minutes.

Auto-deploy is already on: whenever your copy on GitHub changes, Render updates your server by
itself. You don't need to set anything.

### Step 3: Open your server and press Start

1. Click the service. Its address is at the top, like `https://your-app.onrender.com`.
2. Open it **right away** and press **Start**. The first browser to press Start becomes the
   owner, and the page closes to everyone else.
3. The page walks you through the rest, one screen at a time:
   - **Telegram keys:** it shows you exactly where to get them on my.telegram.org.
   - **Log in:** scan a QR code with Telegram on your phone (Settings → Devices → Link Desktop Device).
   - **Agent folder:** make the folder in Telegram and add your chats.
   - **Connect your agent:** you get a block of text to paste into your agent, and a Test button.

That's it.

---

## Everyday use

| You want to… | Do this |
|---|---|
| Share a chat with your agent | Add it to the `Agent` folder in Telegram |
| Take a chat away | Remove it from the folder |
| Manage the server from another browser | Open your server's address and sign in with your `api_id` + `api_hash` (look them up again at my.telegram.org) |
| Get a new agent token | Open your server's address → **New token**, then update your agent |
| Update to the latest version | On GitHub, open your fork → **Sync fork** → **Update branch**. Render redeploys by itself; your login stays |
| Stop everything right now | Telegram → Settings → Devices → end the **Telegram Agent Gateway** session |

---

## Good to know

- **Run one copy only.** Don't scale the server up to two. Two copies on one Telegram login make Telegram end the login.
- **Always use the `https://` address.** The server refuses setup over plain `http://`.
- **Keep your `api_hash` and agent token private.** Whoever has your `api_hash` can manage your server. Whoever has the token can read your folder chats (until you make a new one).
- **Other hosts** (DigitalOcean droplet, Fly, your own server): see [docs/SELF-HOST.md](docs/SELF-HOST.md).
- **How it's secured:** [SECURITY.md](SECURITY.md).

---

## For AI agents helping someone set this up

You're helping a person who may not be technical. Until Step 3 there is **no website yet**: the
setup page only exists once their server is running.

- **Step 1 (their fork):** if you can run commands and `gh` is logged in, do it for them with the
  `gh repo fork …` command above, then tell them the fork is ready. Otherwise they click **Fork**.
- **Step 2 (Render):** all clicks in their browser. You explain, they click.
- **Step 3:** their server's own page explains everything.

**How to help**
- Go one step at a time. Say what to click, in plain words, then wait until they say it's done.
- Use the exact button names from this page (**Fork**, **+ New → Blueprint**, **Deploy Blueprint**, **Start**).
- Ask for a screenshot whenever they're unsure what they're looking at.
- Once they reach Step 3, their server's own page explains everything. Let them follow it and help when they get stuck.

**Never ask them to paste into this chat:** their `api_hash`, Telegram login codes, or their
2FA password. Those go only into their own server's page. The agent block they copy at the end
(address + token) *is* meant for you; that's how you'll read their chats.

**Common snags**

| They say… | What's going on / what to tell them |
|---|---|
| "I'm on a page with Language, Region, Compute…" | That's "New Web Service". Cancel, then **+ New → Blueprint** |
| "A Blueprint file was found, but there was an issue" | Click **Blueprint Path** and choose `render.yaml`, then retry |
| "It wants a card" | Yes: the disk needs the Starter plan (~$7/month). The free plan can't keep the login |
| "This server already has an owner" | Someone pressed Start in another browser. If it was them: sign in with `api_id` + `api_hash`. If not: delete the service in Render and deploy again |
| "my.telegram.org just says ERROR" | Known Telegram quirk. Turn off VPN, try another browser |
| "Telegram asked to wait…" / "paused logins" | Too many login tries. Wait about an hour, then try once |
| "I scanned the QR and nothing happened" | Wait a few seconds; if they have a 2FA password, the next screen asks for it. Don't scan twice |
| "Check folder says it includes chat types" | In Telegram, edit the folder → Add Chats → untick everything under **Chat types** |
| "Open this page over HTTPS" | They used `http://` or an IP address. Use the `https://…onrender.com` address |

**Updating their fork later:** on GitHub, their fork → **Sync fork** → **Update branch**. With a
terminal: `gh repo sync <their-user>/telegram-agent-gateway`. Render redeploys on its own.

**After setup, using the API**

`Authorization: Bearer <token>` on every request, `https://` only. Paths below are relative to `https://<their-server>`.

| Method | Path | What it returns |
|---|---|---|
| GET | `/v1/chats` | The chats in the folder |
| GET | `/v1/chats/{dialog_id}/messages` | Newest first. `limit`, then `before_id` (older) or `after_id` (newer); also `around_id`, `topic_id`, `search`, `media_only` |
| GET | `/v1/chats/{dialog_id}` | Chat info plus `description` and `members_count` |
| GET | `/v1/chats/{dialog_id}/pinned` | Pinned messages (usually the important ones) |
| GET | `/v1/chats/{dialog_id}/messages/{message_id}` | One message, e.g. the one a reply points to |
| GET | `/v1/chats/{dialog_id}/messages/{message_id}/reactions` | Who reacted with what (Telegram only allows this in smaller groups) |
| GET | `/v1/chats/{dialog_id}/members` | Members with name, username and role (owner / admin / member); `q` to search. Telegram may hide this in big groups |
| GET | `/v1/chats/{dialog_id}/topics` | Topic names in forum groups (use `topic_id` with `/messages`) |
| GET | `/v1/search?dialog_id=&q=` | Search inside one chat |
| GET | `/v1/media/{dialog_id}/{message_id}` | The file; add `?thumb=1` for a small preview |
| GET | `/v1/transcript/{dialog_id}/{message_id}` | Text of a voice message or video note. `done`, `pending` (retry after `Retry-After`), or `unavailable` (download `media_path` and transcribe it yourself) |
| GET | `/health` | Server status, no token needed |

Each message has: `text`, `sender_name`, `date`, `edited`, `pinned`, `reply_to_msg_id`, `topic_id`,
`album_id` (screenshots sent together share it), `links` (incl. URLs hidden behind words),
`mentions`, `reactions` (emoji + count), `link_preview`, `views` / `forwards` / `replies`,
`service` (system events like someone joining), and `media` (files, or inline polls, locations,
dice, contacts).

`404` = chat isn't in the folder. `429` = wait `Retry-After` seconds. `503` = the folder needs the owner's attention (tell them to open their server's page).

---

## For developers

```bash
python3.12 -m venv .venv && .venv/bin/pip install -r requirements.txt pytest pytest-asyncio pytest-aiohttp
.venv/bin/python -m pytest
cd src && DATA_DIR=../data PORT=8099 ../.venv/bin/python -m gateway   # open http://localhost:8099
```

Design and security model: [docs/SPEC.md](docs/SPEC.md).
