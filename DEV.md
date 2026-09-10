# Running this blog locally

Personal quick-reference for working on the site. For the full first-time-setup story (why Ruby 3.1, why these env vars, what each step does), see [`_tutorials/jekyll-local-preview.md`](_tutorials/jekyll-local-preview.md).

---

## TL;DR — start the server

**Daily workflow on this Mac** (PATH + locale are wired permanently into `~/.zshrc`):

```bash
cd ~/Desktop/MyBlogPage/AlanLXXL.github.io
bundle exec jekyll serve --livereload
```

Then open **http://127.0.0.1:4000**. Save any file → browser auto-refreshes. `Ctrl+C` to stop.

### When the short version works (and when it doesn't)

| Condition | Works? |
|---|---|
| New iTerm tab + `cd` into project + `bundle exec…` | ✅ Yes |
| Same tab that was open *before* PATH was fixed | ❌ Run `source ~/.zshrc`, or open a fresh tab |
| Forgot to `cd` into the project | ❌ Bundler can't find the Gemfile |
| New Mac / fresh install | ❌ Redo one-time setup in [`_tutorials/jekyll-local-preview.md`](_tutorials/jekyll-local-preview.md) |

### Bulletproof fallback

If anything looks off and you don't want to debug, run this 5-line block — it works regardless of shell state because the env vars are set in the same shell as the server:

```bash
cd ~/Desktop/MyBlogPage/AlanLXXL.github.io
export PATH="/opt/homebrew/opt/ruby@3.1/bin:$PATH"
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
bundle exec jekyll serve --livereload
```

---

## How this all fits together

The site is **Jekyll**, a static-site generator written in Ruby. The stack has three layers:

**1. Ruby — the language.**
macOS ships with Ruby 2.6 at `/usr/bin/ruby`. Apple froze it in 2022 and won't update it, so it's too old for modern Jekyll. But the newest Ruby (3.2+) is *too new* — Jekyll 3.9 (pinned by GitHub Pages) still calls APIs that newer Ruby removed (`tainted?`, plus stdlib modules like `csv` and `webrick` dropped in 3.4+). **Ruby 3.1, installed via Homebrew at `/opt/homebrew/opt/ruby@3.1/`, is the only version that threads the needle.** That's why our PATH points there.

**2. Bundler — the dependency manager.**
Bundler is Ruby's equivalent of `npm` (Node) or `pip` (Python). It reads the [`Gemfile`](Gemfile), resolves a compatible set of library versions, and installs them into `./vendor/bundle/` (project-local, not system-wide). The `bundle exec` prefix means *"run this command using exactly this project's library versions, not whatever happens to be on the system."* That's why every Jekyll command starts with `bundle exec`.

The "bundle" itself is just the locked, reproducible set of gem versions for this project. Same idea as a `package-lock.json` or `poetry.lock`.

**3. Jekyll + plugins — the site generator.**
The actual site-building logic. Declared in the `Gemfile` (`github-pages`, `jekyll-feed`, `jekyll-paginate`, `jekyll-sitemap`, …), installed by Bundler, invoked via `bundle exec jekyll serve`. GitHub Pages runs the same `github-pages` gem on their servers when you push, so what you see locally is what goes live.

**Worktrees note.** Each git worktree (`master`, `claude/…`) maintains its own `vendor/bundle/` — Bundler resolves gems relative to the working directory, so installing on one worktree doesn't share with another. If you create a new worktree and `bundle exec` errors out, run `bundle install` once inside it.

---

## One-time setup (already done on this machine)

| Step | Status |
|---|---|
| Homebrew installed | ✅ |
| `ruby@3.1` installed via brew | ✅ |
| PATH + locale wired in `~/.zprofile` **and** `~/.zshrc` | ⚠️ verify in a **new terminal** — `which ruby` should say `/opt/homebrew/opt/ruby@3.1/bin/ruby`. If it says `/usr/bin/ruby`, the exports got wiped. On this machine they must live in `.zshrc` (not just `.zprofile`) because `.zshrc` line 2 does a destructive `export PATH=<hardcoded list>` with no `:$PATH` on the end — it overwrites whatever `.zprofile` set. The Ruby exports go at the **end** of `.zshrc` so nothing earlier can clobber them. |
| `bundle config set path vendor/bundle` | ✅ (see `.bundle/config`) |
| `bundle install` | ✅ (see `vendor/bundle/`) |

If you ever move to a new Mac, walk through [`_tutorials/jekyll-local-preview.md`](_tutorials/jekyll-local-preview.md) once.

---

## Common workflows

### Add a new blog post

1. Create a file in `_posts/` named `YYYY-MM-DD-slug.md`.
2. Front matter template:
   ```yaml
   ---
   title: "Your Title"
   date: 2026-05-09
   last_modified_at: 2026-05-09T23:59:00+08:00
   categories:
     - Blog
   tags:
     - Reflection
   excerpt: "One-sentence summary for the post listing."
   layout: single
   author_profile: true
   read_time: true
   comments: true
   share: true
   related: true
   ---
   ```
3. Save → livereload picks it up. No restart needed.

### Add a new tutorial

1. Create a file in `_tutorials/` named `slug.md` (no date prefix — tutorials aren't dated).
2. Front matter:
   ```yaml
   ---
   title: "Tutorial Title"
   excerpt: "One-line summary."
   app: "Tool / Stack"
   difficulty: "Beginner | Intermediate | Advanced"
   time: "30 minutes"
   last_tested: 2026-05-09
   tags:
     - SomeTag
   ---
   ```
3. The tutorial automatically appears at `/tutorials/`.

> **Note (Sept 2026):** Tutorials is no longer linked from the top nav. The section still builds at `/tutorials/`; its menu entry is kept commented out in [`_data/navigation.yml`](_data/navigation.yml) if you want it back.

### Add a new project

Projects live in the `_projects/` collection. Each one gets its own page at `/projects/<slug>/`, a card on [`/projects/`](_pages/projects.md), and the top three (by `order`) also show on the homepage under "Selected projects".

1. Copy [`_projects/TEMPLATE.md`](_projects/TEMPLATE.md) to `_projects/<slug>.md` — the slug becomes the URL.
2. Fill in the front matter and **remove the `published: false` line**:
   ```yaml
   ---
   title: "Project Name"
   order: 1                          # 1 = shown first
   period: "Jan – May 2026"
   role: "ML engineer · team of 3"   # or "Solo project"
   excerpt: "One sentence: what it does and why it matters."
   result: "One concrete outcome, ideally with a number."
   stack: [Python, PyTorch, FastAPI]
   links:
     - label: "GitHub"
       url: "https://github.com/AlanLXXL/repo-name"
   # image: /assets/images/projects/<slug>.png   # optional card thumbnail
   ---
   ```
3. Write the body: Problem → What I built → Results → What I learned. Headings feed the sticky table of contents.
4. Save → livereload picks it up. Cards sort by `order`, lowest first.

The card markup lives in [`_includes/project-card.html`](_includes/project-card.html), the page layout in [`_layouts/project.html`](_layouts/project.html), and the styles in [`_includes/head/custom.html`](_includes/head/custom.html).

### Reorder the top nav

Edit [`_data/navigation.yml`](_data/navigation.yml). Order of entries = order in the menu.

### Change site title, author bio, theme

Edit [`_config.yml`](_config.yml). **Requires a server restart** (Ctrl+C, re-run).

### Add an image

1. Drop the file in `assets/images/`.
2. Reference it in markdown:
   ```markdown
   ![alt text](/assets/images/filename.png)
   ```
   Or, for posts with multiple images, the reference-link style used in `2025-06-30-rockmusic.md`:
   ```markdown
   ![][image1]

   [image1]: /assets/images/rockmusic-image1.png
   ```

---

## Deploying

This blog is hosted on **GitHub Pages**. There is no separate deploy step:

```bash
git add <files>
git commit -m "your message"
git push
```

GitHub rebuilds the site automatically (1–3 minutes). Local preview = what you'll see on the live site.

The live URL: https://alanlxxl.github.io/

---

## When things break

| Symptom | Likely cause | Fix |
|---|---|---|
| `Could not find 'bundler' (X.Y.Z)` | Wrong Ruby active (system 2.6 instead of brew 3.1) | **Diagnose:** `which ruby` — if it's `/usr/bin/ruby`, PATH is wrong. **Quick fix (this terminal only):** `export PATH="/opt/homebrew/opt/ruby@3.1/bin:$PATH"`. **Permanent fix:** open `~/.zprofile`, make sure the Ruby line says `opt/ruby@3.1/bin` (not `opt/ruby/bin`), then `source ~/.zprofile` or open a new terminal. |
| `Invalid US-ASCII character "\xE2"` | UTF-8 locale not set | `export LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8` |
| `cannot load such file -- csv` (or similar stdlib gem) | Ruby is too new (3.4+) | Make sure `ruby@3.1` is what `which ruby` returns |
| Port 4000 already in use | A previous `jekyll serve` is still running | `lsof -ti:4000 \| xargs kill` or `bundle exec jekyll serve --port 4001` |
| Changes to `_config.yml` aren't showing | Config is read once at startup | Restart the server (Ctrl+C, re-run) |
| Livereload not refreshing | Browser cached the old WebSocket | Hard refresh (Cmd+Shift+R) or restart server |

---

## Useful commands

```bash
# Reset everything (rebuild from scratch, useful if cache is corrupt)
rm -rf _site/ .jekyll-cache/
bundle exec jekyll serve --livereload

# Faster rebuilds once you have many posts
bundle exec jekyll serve --livereload --incremental

# Build without serving (just generate _site/)
bundle exec jekyll build

# Update gems (rarely needed — only if something breaks after a Ruby upgrade)
bundle update
```

---

## Project layout — what lives where

```
AlanLXXL.github.io/
├── _config.yml          ← site-wide settings (title, author, plugins) — restart on change
├── _data/
│   └── navigation.yml   ← top nav order
├── _includes/           ← reusable Liquid fragments (about-content.md, etc.)
├── _pages/              ← standalone pages (about, projects listing, archives)
├── _posts/              ← blog posts, filename = YYYY-MM-DD-slug.md
├── _projects/           ← project write-ups (collection) — copy TEMPLATE.md to start one
├── _tutorials/          ← tutorial collection, no date prefix (unlinked from nav, still builds)
├── assets/
│   └── images/          ← all images go here
├── index.md             ← homepage (just renders the about content)
├── Gemfile              ← Ruby dependencies — touch only if you know why
├── DEV.md               ← this file
└── README.md            ← upstream theme readme (mostly boilerplate)
```
