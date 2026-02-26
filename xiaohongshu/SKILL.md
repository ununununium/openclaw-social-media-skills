---
name: xiaohongshu
description: Post image-based notes (图文笔记) to 小红书 (Xiaohongshu/RedNote) using md2red to convert markdown to images, handle 互动维护 (notifications, replies, follow-back), and report stats. Use when posting to 小红书, running the md2red workflow, doing 互动维护, or checking 小红书 account stats.
---

# 小红书 (Xiaohongshu / RedNote) Posting Skill

## Setup

Configure these before first use:

- **md2red script path:** path to your `md2red.py` install (see [md2red](https://github.com/your-org/md2red))
- **Browser:** `profile=openclaw` only — already logged in. **Never use Chrome relay** (CDP disconnects on Xiaohongshu page navigation)
- **Content plan:** a markdown file in your workspace (e.g. `xiaohongshu-content-plan.md`)
- **Account profile ID:** your Xiaohongshu profile ID (visible in profile URL)

## 【1. 发布新帖 — Publishing a Post】

**Step 1 — 选题 (Pick a topic)**

Read your content plan file, find the post scheduled for this time slot.

**Step 2 — 写 markdown (Write content)**

- Short paragraphs, bold key sentences, natural call-to-action at the end
- ⚠️ **No `#tags` in the markdown file** — md2red renders markdown as images; hashtags will appear inside the image
- Tags go only in Step 7's body innerHTML

**Step 3 — 生成图片 (Generate images)**

Save your markdown to the md2red input file, then run:

```bash
cd /path/to/md2red && python3 md2red.py input.md --output <name>
```

Images output to `output/<name>/` (1.png, 2.png, …)

**Step 4 — 打开上传页 (Open upload page)**

```
browser navigate profile=openclaw targetUrl=https://creator.xiaohongshu.com/publish/publish?from=tab_switch&target=image
```

Wait 3 seconds.

**Step 5 — 上传图片 (Upload images)**

- Take a snapshot → find the "上传图片" (Upload Image) button ref
- `browser upload ref=<button-ref> paths=[1.png, 2.png, ...]`
- ⚠️ Do **not** use `selector="input[type=file]"` — it's hidden and won't trigger the upload. Must use the button ref so Playwright intercepts the fileChooser event correctly
- Wait 5 seconds

**Step 6 — 填标题 (Fill in title)**

- Snapshot → find the title textbox → click + type
- **≤20 characters**

**Step 7 — 填正文 (Fill in body)** — must use JS evaluate, not `type`

```js
(function () {
  var el = document.querySelector(".tiptap.ProseMirror");
  if (!el) {
    var eds = document.querySelectorAll("[contenteditable=true]");
    el = eds[eds.length - 1];
  }
  el.focus();
  el.innerHTML = "<p>Your body text here</p><p><br></p><p>#tag1 #tag2</p>";
  el.dispatchEvent(new Event("input", { bubbles: true }));
  return "done";
})();
```

**Step 8 — 发布 (Publish)**

- Click the "发布" button
- Success indicator: page returns to the initial video upload page

**Step 9 — 标记已发布 (Mark as published)**

- Update your content plan file, mark the post as published

---

## 【2. 互动维护 — Engagement】

Using `profile=openclaw` browser:

1. Open https://www.xiaohongshu.com/notification — review all notifications
2. For each comment/reply, write a quality response (show expertise, avoid templated replies)
3. For new follower notifications → visit their profile and follow back if relevant

---

## 【3. 数据汇报 — Stats Check】

Using `profile=openclaw` browser:

1. Open your profile page and note current follower count
2. Check the most recent post's likes and saves
3. Self-review: which content types perform better, how to adjust next time

---

## 【汇报格式 — Report Format】

At the end of each session, report:

- Title of post published this session
- Number of comments replied to
- Number of accounts followed back
- Current follower count
- Likes / saves on last post
- Content optimization thoughts for next time

---

## ⚠️ Content Moderation Guidelines

Xiaohongshu has strict automated review. Adapt these based on your niche, but general principles:

- Avoid sensitive financial/regulatory terms specific to your topic
- Use neutral / professional synonyms instead of platform-specific branded terms
- When in doubt, be vague rather than precise with anything compliance-adjacent
- Tags are also reviewed — avoid anything likely to trigger financial, political, or adult content filters
