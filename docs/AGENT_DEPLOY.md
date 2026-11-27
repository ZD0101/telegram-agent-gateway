# Getting an AI agent to help with setup

Two ways, depending on your agent.

## A. "Walk me through it" (any agent: Claude, ChatGPT, Grok…)

Most people want this. The agent explains each step and you click. Paste:

```text
Help me set up Telegram Agent Gateway. Read the README at
https://github.com/ZD0101/telegram-agent-gateway
(the section "For AI agents helping someone set this up" is for you).

Walk me through it one step at a time in plain, simple words: what to click, then wait for me to
say done. I'm using Render and I'll sign in to everything with GitHub. Never ask me to paste my
api_hash, Telegram codes or 2FA password into this chat; those go only into my server's own page.
If I get stuck, ask me for a screenshot.
```

If your agent can't open links, paste the README's text in instead.

## B. "Deploy it for me" (coding agents with a terminal: Claude Code, Cursor…)

Only for your own server (VPS, Docker, Fly) where the agent can run commands. For Render, use A:
Render is all clicks in the browser.

```text
Deploy Telegram Agent Gateway from my copy (https://github.com/YOUR_GITHUB_USER/telegram-agent-gateway)
as a single-tenant service I own. Follow docs/SELF-HOST.md.

Rules:
1. Persistent storage at /data is mandatory (the Telegram login lives there).
2. Run EXACTLY ONE instance: no replicas, no scaling, no zero-downtime or blue-green deploys.
   Two copies on one login make Telegram end it. Fly: launch with --ha=false.
3. HTTPS ONLY. Give me an https:// address, never a bare IP or http://. On a VPS use the
   deploy/digitalocean stack (Caddy, automatic certificate on <ip>.sslip.io) or Caddy with my
   domain. Do not set ALLOW_PLAIN_HTTP.
4. Don't set any secrets. The setup page collects everything and stores it on /data.
5. Never commit secrets, session files, api_hash or tokens to git.
6. Check that GET https://<address>/health returns JSON.
7. Then give me the https:// address straight away and STOP. Do not open the setup page:
   the first browser to press Start becomes the owner, and that must be me. I do the
   Telegram login myself.
```
