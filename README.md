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
npx publishport login        # prints an authorization link — open it, click Authorize
npx publishport login --wait # picks up the credential once you have approved
npx publishport capabilities # which of your accounts are connected, on which machine
```

The agent runs these itself: it hands you the link, you approve in the browser, and it carries on.
No endpoint to copy, no token to paste.

Both routes go through the same relay, so the agent does not have to run on the machine that
publishes — claude.ai, a phone or a server works just as well.

Setup guide: [publishport.app/docs/agent-skill](https://publishport.app/docs/agent-skill)

## What it looks like

> "Turn this changelog into a launch post and publish it to X, Reddit r/SideProject and Dev.to."

The agent checks which of those accounts are signed in, reads each platform's publishing schema,
writes the body to a file, shows you the three drafts, publishes them one at a time, and comes
back with the three live links.

## 中文说明

PublishPort Skill 让 Claude Code、Codex、Cursor、WorkBuddy、OpenClaw、Gemini CLI 等 AI 编程助手，
直接用你电脑上已经登录的浏览器，把内容发到小红书、抖音、快手、微信视频号、微信公众号、B 站、知乎、
微博、今日头条、百家号、搜狐号、掘金、CSDN、豆瓣、即刻、番茄小说、七猫小说等国内平台，以及 X、
LinkedIn、Reddit、YouTube、TikTok、Medium、WordPress 等海外平台。不需要任何平台的 API Key，
发布走的是你自己的账号、你自己的网络。

接法：

1. 安装 [PublishPort 客户端](https://publishport.app/zh)（macOS / Windows / Linux），在里面登录要发的平台。
2. 把这个 skill 装给你的 AI：`npx skills add karuha-m/publishport-skill`。
3. 对 AI 说一句「用 PublishPort 帮我把这篇发到小红书和知乎」。AI 会先给你一个授权链接，点开授权，
   它就接着发，发完把链接回给你。

除了单次发布，也能让 AI 定时发、一稿多发、回评论和私信。教程：[publishport.app/zh/docs](https://publishport.app/zh/docs)

## License

MIT — see [LICENSE](LICENSE).
