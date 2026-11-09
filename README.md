# Buzzmark MCP server

Buzzmark is a ghostwriter that writes, plans and schedules social media posts for founders and growing businesses. It reads your website and products first, so the first post already sounds like you, and keeps learning from the posts you've published and the way you edit its drafts. A founder gets a voice of their own, separate from the company's.

This is Buzzmark's remote MCP server. Connect it to VS Code, Copilot, Claude, ChatGPT, Cursor or any app that speaks MCP, and ask for posts for Instagram, LinkedIn, Facebook, X, Threads or TikTok without leaving it.

- Server URL: `https://buzzmark.ai/mcp`
- Transport: Streamable HTTP
- Sign-in: OAuth 2.1 with PKCE and dynamic client registration. There is no key to copy.
- Registry name: `ai.buzzmark/mcp`

The server is hosted by Buzzmark. This repository holds its description, not its source.

## What you can ask

- "Find me post ideas from this week's news in my sector."
- "Write 3 LinkedIn posts about what we shipped this week, then give X its own version."
- "How much does this post sound like my brand? Fix what doesn't."
- "Plan next week around my best posting times and schedule it."
- "Make a picture for this post."
- "What worked on Instagram this month?"

Every post is checked against Buzzmark's writing rules before you see it.

## Before you start

You need a Buzzmark account with a plan and at least one social account connected in Buzzmark. New accounts get 7 days free: [sign up at buzzmark.ai](https://buzzmark.ai/signup).

## Connect

### VS Code and GitHub Copilot

Add this to `.vscode/mcp.json` in your workspace, or to your user MCP configuration:

```json
{
  "servers": {
    "buzzmark": {
      "type": "http",
      "url": "https://buzzmark.ai/mcp"
    }
  }
}
```

Start the server. VS Code opens a Buzzmark page the first time: sign in and allow it. Then ask Copilot in agent mode: "What social accounts do I have in Buzzmark?"

### Other apps

Any app that speaks MCP over Streamable HTTP connects with the same URL, `https://buzzmark.ai/mcp`. Apps with OAuth sign in on their own. An app without sign-in can send a personal API key as a Bearer header: make one on the AI apps page in Buzzmark, and revoke it there when you're done.

Steps for Claude, ChatGPT, Grok, Cursor and Claude Code are on [buzzmark.ai/ai-apps](https://buzzmark.ai/ai-apps).

## Agent skill

[`skills/buzzmark/SKILL.md`](skills/buzzmark/SKILL.md) teaches an AI agent to connect this server and use its tools well: which tool to start with, how to write in your brand's voice, first comments, tagging and scheduling. Install it with:

```bash
npx skills add piiklabs/buzzmark-mcp --skill buzzmark
```

Or from [ClawHub](https://clawhub.ai/buzzmark/skills/buzzmark) for OpenClaw:

```bash
openclaw skills install @buzzmark/buzzmark
```

## Install in Hermes Agent

This repository is also a portable Agent Plugins v1 package for [Hermes Agent](https://hermes-agent.nousresearch.com): `plugin.json`, `mcp.json` and the skill in `skills/buzzmark/`.

```bash
hermes plugins install buzzmark --enable
```

The first time Hermes uses a Buzzmark tool, your browser opens a Buzzmark page: sign in and allow it. Then ask Hermes: "What social accounts do I have in Buzzmark?"

What the plugin does on your machine:

- Every tool call goes to Buzzmark's hosted server at `buzzmark.ai` and acts on the Buzzmark account you sign in to. The tools come from that server, so they can change without a new version of this package.
- Sign-in runs `mcp-remote@0.1.49` through `npx` (Node.js and npm required). It opens your browser and a local callback port the first time, and keeps the sign-in under `~/.mcp-auth`. There is no key in the package or in Hermes's config.
- The package marks the server `untrusted`, so Hermes asks before every tool that isn't read-only (saving a draft, scheduling, making a picture) and refuses those in unattended runs. Writing posts and making pictures use your plan's monthly Buzz and pictures.
- Nothing posts right away. Scheduled posts stay on your Buzzmark calendar, where you can change or remove them before they go out.
- No code, binaries, telemetry or self-updater in the package.

On a machine without a browser, skip the plugin and add the server to Hermes's `config.yaml` with a personal API key from the AI apps page in Buzzmark, kept in `~/.hermes/.env` as `BUZZMARK_API_KEY`:

```yaml
mcp_servers:
  buzzmark:
    url: "https://buzzmark.ai/mcp"
    headers:
      Authorization: "Bearer ${BUZZMARK_API_KEY}"
    trust: untrusted
```

## What your AI app can do in your account

Buzzmark never posts on the spot. Your AI app can save drafts and schedule, move or unschedule posts, and every scheduled post stays on your Buzzmark calendar, where you can change or remove it before it goes out.

Your AI app works only with the brands, accounts and posts in the Buzzmark account you sign in to. Disconnect it any time from the AI apps page in Buzzmark.

## Tools

Every tool, its inputs and the sign-in details are at [buzzmark.ai/ai-apps/mcp](https://buzzmark.ai/ai-apps/mcp), generated from the server's own code.

## Help

- Setup guides: https://buzzmark.ai/ai-apps
- Developer reference: https://buzzmark.ai/ai-apps/mcp
- Support: support@buzzmark.ai
- Privacy Policy: https://buzzmark.ai/legal/privacy
- Terms and Conditions: https://buzzmark.ai/legal/terms

Buzzmark is made by Piik Labs AB in Göteborg, Sweden.
Create a buzz around your brand.
