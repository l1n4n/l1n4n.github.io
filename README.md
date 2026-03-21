# Thoughts — A minimal Jekyll blog for GitHub Pages

A clean, no-fuss personal site for posting short thoughts and articles. Hosted free on GitHub Pages.

## 🚀 Setup (5 minutes)

### 1. Create your GitHub repo

Go to [github.com/new](https://github.com/new) and create a repo named **`<your-username>.github.io`** (replace with your actual GitHub username). This gives you a site at `https://<your-username>.github.io`.

### 2. Push these files

```bash
cd jekyll-blog
git init
git add .
git commit -m "Initial site"
git remote add origin https://github.com/<your-username>/<your-username>.github.io.git
git push -u origin main
```

### 3. Enable GitHub Pages

Go to your repo → **Settings** → **Pages** → set source to **"Deploy from a branch"** → select **main** branch → Save.

Your site will be live in ~1 minute at `https://<your-username>.github.io`.

### 4. Customize

Edit `_config.yml` to change:
- `title` — your site name
- `description` — the tagline under the title
- `author.name` — your name

---

## ✍️ How to post a new thought

### Option A: GitHub web UI (easiest, works from phone)

1. Go to your repo on GitHub
2. Navigate to `_posts/`
3. Click **"Add file" → "Create new file"**
4. Name it: `YYYY-MM-DD-your-title-here.md` (e.g. `2026-03-22-shower-thought.md`)
5. Paste this template:

```markdown
---
layout: post
title: "Your title here"
---

Your thought goes here. Just write in plain text or markdown.
```

6. Click **"Commit changes"** — the site rebuilds in ~30 seconds.

### Option B: Ask Claude to write it

Come to Claude (this chat!) and say something like:

> "Write me a short blog post about [your topic]. Format it as a Jekyll post with the front matter."

Copy the output, paste it as a new file in `_posts/` via the GitHub web UI.

### Option C: Claude Code (most automated)

If you have Claude Code installed:

```bash
claude "Write a short thought about [topic] and save it as a Jekyll post in _posts/"
git add . && git commit -m "New post" && git push
```

---

## 📁 File structure

```
.
├── _config.yml          ← Site settings
├── _layouts/
│   ├── default.html     ← Base page template
│   └── post.html        ← Single post template
├── _posts/              ← YOUR POSTS GO HERE
│   └── 2026-03-21-example.md
├── assets/css/
│   └── style.css        ← All styling
├── index.html           ← Homepage (auto-lists posts)
├── Gemfile              ← Dependencies
└── README.md
```

## 🎨 Customization

- **Colors**: Edit the CSS variables at the top of `assets/css/style.css`
- **Fonts**: Change the Google Fonts import in `_layouts/default.html`
- **Dark mode**: Automatic via `prefers-color-scheme` — edit the dark-mode variables in the CSS

## Post formatting tips

Posts are plain Markdown. You can use:

- **Bold** and *italic* text
- [Links](https://example.com)
- `inline code`
- Block quotes with `>`
- Images: `![alt text](url)`
- Headings with `##` and `###`

That's it. Keep it simple, keep it yours.
