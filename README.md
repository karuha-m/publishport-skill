# PublishPort Skill

An Agent Skill that lets your coding agent publish content to **60+ social and content platforms** —
from the browser you are already logged into, on your own machine.

X · Reddit · LinkedIn · Medium · Dev.to · Hashnode · Substack · Bluesky · Mastodon · Threads ·
Telegram · Tumblr · Pinterest · YouTube · TikTok · Xiaohongshu (RedNote) · Douyin · Bilibili ·
Zhihu · Weibo · WeChat — the live list is at
[api.publishport.app/api/registry](https://api.publishport.app/api/registry).

## Why through a browser

Most publishing tools stop where the official APIs stop, which leaves out the platforms people
actually care about: Xiaohongshu, Douyin, Zhihu and WeChat have no public write API at all, and
the ones that do tend to gate posting behind an app review.

PublishPort takes the other route: commands run on your machine, in your own logged-in browser
session, from your own IP. Nothing is proxied through someone else's account, and there are no API
keys to provision — you sign in once per platform in a normal browser window, and that session is
what publishes.

This skill teaches the agent how to drive that setup properly: read each platform's real command
schema instead of guessing flags, stage long text and media as files, show you the post before it
goes out, and verify a post landed instead of blindly retrying and posting twice.

## Install

**Any agent (recommended)** — works with Claude Code, Cursor, Codex, Copilot, Gemini, OpenCode and
70+ others via [skills.sh](https://skills.sh):

```bash
npx skills add karuha-m/publishport-skill
```

Add `-g` to install globally instead of into the current project, or `-a claude-code` to target one
specific agent.

**Claude Code — as a plugin:**

```
/plugin marketplace add karuha-m/publishport-skill
/plugin install publishport@publishport
```

**By hand — just the file:**

```bash
mkdir -p ~/.claude/skills/publishport
curl -fsSL https://raw.githubusercontent.com/karuha-m/publishport-skill/main/skills/publishport/SKILL.md \
  -o ~/.claude/skills/publishport/SKILL.md
```

**On claude.ai:** download the latest `publishport-skill.zip` from
[Releases](https://github.com/karuha-m/publishport-skill/releases) and upload it under
Settings → Capabilities → Skills.

## Requirements

The skill is the instructions; the publishing itself needs the **PublishPort desktop app**
([publishport.app](https://publishport.app), macOS / Windows / Linux). Install it, sign in to the
platforms you want, and your agent can publish to them — over PublishPort's MCP endpoint if it
speaks MCP, otherwise with the official CLI:

```bash
npx publishport login        # paste the access endpoint from the app's Connect AI panel
npx publishport capabilities # which of your accounts are connected, on which machine
```

Both routes go through the same relay, so the agent does not have to run on the machine that
publishes — claude.ai, a phone or a server works just as well.

Setup guide: [publishport.app/docs/agent-skill](https://publishport.app/docs/agent-skill)

## What it looks like

> "Turn this changelog into a launch post and publish it to X, Reddit r/SideProject and Dev.to."

The agent checks which of those accounts are signed in, reads each platform's publishing schema,
writes the body to a file, shows you the three drafts, publishes them one at a time, and comes
back with the three live links.

## License

MIT — see [LICENSE](LICENSE).
