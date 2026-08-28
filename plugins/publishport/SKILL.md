---
name: publishport
description: Publish, cross-post and schedule content on social and content platforms — X/Twitter, Reddit, LinkedIn, Medium, Dev.to, Hashnode, Substack, Bluesky, Mastodon, Threads, Telegram, Tumblr, Pinterest, YouTube, TikTok, plus Chinese platforms such as Xiaohongshu (RedNote), Douyin, Bilibili, Zhihu, Weibo and WeChat — through the user's own logged-in browser via PublishPort. Also covers checking which accounts are connected, attaching images or video to a post, and confirming a post actually went live. Use whenever the user asks to post, publish, cross-post, repost or schedule content, asks which of their platform accounts are connected, or asks to pull back the link or stats of something they published.
license: MIT
---

# Publishing with PublishPort

PublishPort runs publishing commands **on the user's own machine, inside their own logged-in
browser**. No platform API keys, no cloud account impersonation — every action comes from the
user's real session, which is why platforms with no public write API (Xiaohongshu, Douyin,
Zhihu, WeChat…) can be published to at all.

Two rules matter more than everything else below:

- **Never guess a flag.** Ask the CLI for its schema; it is the only accurate source.
- **Never publish twice.** A command that errors may still have posted. Check, then retry.

## 1. Pick the channel

Work through these in order:

**a. PublishPort MCP tools are available** (`list_capabilities`, `local_bash`, `publish_guide`, …)
— use them and follow their own instructions, which are authoritative and more detailed than this
file. Call `list_capabilities` first: it returns the online devices, the live per-platform login
state, and the `device` handle every other tool needs. Then call `publish_guide(<platform>,
<device>)` before publishing — it returns that platform's fast path together with the live
`--help` schema, saving you a round trip.

**b. No MCP, but the desktop app is installed** — run its bundled `ppcli` yourself. The app keeps
its runtime in its own data directory and deliberately never modifies the user's `PATH`, so
resolve the binary once and reuse the absolute path:

```bash
command -v ppcli \
  || ls ~/"Library/Application Support/app.publishport.desktop/runtime/bin/ppcli" \
  || ls ~/.local/share/app.publishport.desktop/runtime/bin/ppcli
```

On Windows it is `%APPDATA%\app.publishport.desktop\runtime\bin\ppcli.cmd`. Keep the desktop app
running while you work — it owns the browser the commands drive.

**c. Neither** — stop and tell the user what is missing: publishing needs the PublishPort desktop
app (<https://publishport.app>), where they sign in to each platform once, in a normal browser
window. Do not substitute scraping, headless logins, or unofficial platform APIs.

## 2. Publish

1. **Confirm the account.** `ppcli auth status` lists login state per platform, but it is cached
   and can be stale — before a real publish run `ppcli <platform> whoami`. If that reports
   `AUTH_REQUIRED`, ask the user to sign in again in the desktop app instead of pushing ahead.
2. **Read the real schema.** `ppcli <platform> --help`, then `ppcli <platform> <command> --help`.
   Required arguments differ per platform (some need a cover image, a topic, a category id).
   Fill in every required argument deliberately; defaults are rarely what the user wants.
3. **Stage the content as files.** Write the body to a file and pass it with `--file`
   (or the command's own `--<name>-file`). Never inline a long body in the command line: quoting
   breaks on the first apostrophe, and long multi-line text gets truncated on Windows. Images and
   video go in as local absolute paths.
4. **Show the user what you are about to post** — the text, the target platform, and the account —
   and get a yes before the first publish of a session. After that, a summary per post is enough.
5. **Run one command per action.** Give uploads a generous timeout (video can take minutes). If you
   need to wait between steps, wait in a separate command rather than chaining with `&&`.
6. **Verify.** Use the returned URL or id, or re-query the platform's own listing, and hand the
   user a link they can click. A publish is not done until you have seen it exist.

Cross-posting the same piece to several platforms means repeating steps 2–6 per platform: length
limits, required media and tag rules genuinely differ, so re-read each one's schema instead of
reusing the previous argument set.

## 3. When something goes wrong

- **A publish command failed or timed out.** Do not re-run it. Query the platform's latest posts
  or the user's profile first; only publish again once you have confirmed the content is not
  there. Blind retries are the number one cause of duplicate posts.
- **Missing media.** If the task needs an image or video and none was provided, say so and stop.
  Do not go looking through the user's disk for something that might fit.
- **Browser commands keep timing out** (even `whoami`): retry once with `--site-session ephemeral`
  before the subcommand. If everything still times out, ask the user to restart the desktop app.
- **The platform is not in the list.** Say so plainly. The catalogue of supported platforms is
  public at <https://api.publishport.app/api/registry>, and the account only works for platforms
  the user has actually signed into.

## Pacing

These are the user's real accounts, on platforms that do police automation. Publish at a human
rhythm — space posts out, don't fire the same text at a dozen accounts on one platform, and don't
work around a rate limit or a cooldown by retrying in a loop. If a platform pushes back, report it
and stop; getting the account restricted costs the user far more than a delayed post.
