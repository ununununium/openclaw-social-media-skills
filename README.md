# openclaw-social-media-skills

A collection of [OpenClaw](https://openclaw.ai) agent skills for social media automation.

## Skills

### [`x-posting`](./x-posting/SKILL.md)

Post, reply, follow, and engage on X (Twitter). Designed for consistent voice and tone across cron sessions — covers original posts, reply strategy, session logging, and what *not* to do.

**Features:**
- Session startup checklist (log review → notifications → engagement)
- Voice & tone guide (practitioner, not promoter)
- Product/project mention guidelines (organic, never as CTA)
- Persistent session logging in `memory/x-post-log.md`

### [`xiaohongshu`](./xiaohongshu/SKILL.md)

Post image-based 图文笔记 to 小红书 (Xiaohongshu / RedNote) using [md2red](https://github.com/your-org/md2red) to convert markdown to images. Also covers 互动维护 (notifications, replies, follow-backs) and stats reporting.

**Features:**
- Full end-to-end posting workflow (markdown → images → upload → publish)
- Critical upload workaround: must use button ref, not hidden file input
- Body text must be set via JS `evaluate`, not `type` (ProseMirror editor)
- Engagement and follow-back workflow
- Content moderation guidelines

## Usage

Install into your OpenClaw skills directory:

```bash
cp -r x-posting ~/.openclaw/skills/
cp -r xiaohongshu ~/.openclaw/skills/
```

Then configure per-skill setup (account handles, file paths, content plan) as described in each `SKILL.md`.

## Requirements

- [OpenClaw](https://openclaw.ai) with browser tool (`profile=openclaw`)
- For xiaohongshu: [md2red](https://github.com/your-org/md2red) installed locally

## License

MIT
