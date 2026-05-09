---
title: "From `git push` Wait to Live Reload: Local Jekyll on macOS"
excerpt: "How I stopped waiting 1–3 minutes for every blog change to deploy. The setup is finicky the first time — here's the path that actually works in 2026, including the gotchas no one warns you about."
app: "Jekyll / Ruby / Homebrew"
difficulty: "Intermediate"
time: "30 minutes"
last_tested: 2026-05-09
tags:
  - Jekyll
  - Ruby
  - macOS
  - Productivity
---

## Why bother

Every time I changed something on this blog, the loop was: edit → `git push` → wait 1–3 minutes for GitHub Pages to rebuild → refresh and see the result. Multiply that by 20 edits in one evening and you've burned half your time staring at a deploy log.

A local preview at `http://127.0.0.1:4000/` builds the **same site** on your Mac. Save a file, the browser refreshes in under a second. One-time setup; permanent payoff.

This guide covers the setup that actually works as of 2026, including the version traps that wasted me an hour on the first attempt.

## The mental model — three layers, one trap

Jekyll is a Ruby program. To run it, three layers have to work:

1. **Ruby** — the language Jekyll is written in
2. **Bundler** — Ruby's package manager (think `pip` + `venv` rolled into one)
3. **Jekyll + plugins** — the actual site generator

The trap is that **macOS ships with Ruby 2.6**, which Apple froze in 2022. Modern gems require Ruby 3.0+. So we need a newer Ruby installed *alongside* the system one (never replacing it).

But there's a second trap most tutorials miss: **don't install the latest Ruby either.** GitHub Pages pins `Jekyll 3.9` (a 2020 library) which doesn't work on Ruby 3.2+. The sweet spot is **Ruby 3.1** — the last version that works with the GitHub Pages stack.

## Step 1 — Install Homebrew

[Homebrew](https://brew.sh) is the macOS package manager. It installs into `/opt/homebrew/` (a user-owned directory) and never touches `/usr/` or any system files.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

It will ask for your Mac password once (for `sudo`). When it finishes, copy and run the two `eval` lines it prints — they add `brew` to your shell's PATH.

Verify:
```bash
brew --version
```

## Step 2 — Install Ruby 3.1 (specifically)

```bash
brew install ruby@3.1
```

Why exactly 3.1?

- Ruby **3.2** (Dec 2022) removed `tainted?` — a method Liquid 4.0.3 (pinned by github-pages) still calls
- Ruby **3.4** (Dec 2024) removed `csv`, `bigdecimal`, `ostruct` and others from default gems
- Ruby **3.1** has all of these and works cleanly with the old Jekyll stack

After install, `ruby@3.1` is installed as **keg-only** — meaning it's not in your PATH automatically (so it doesn't override macOS Ruby). We add it explicitly next.

## Step 3 — Wire PATH in `.zprofile`

zsh has two startup files:
- **`.zprofile`** runs once at login
- **`.zshrc`** runs for every interactive shell

PATH should be set once at login, so it goes in `.zprofile`:

```bash
echo 'export PATH="/opt/homebrew/opt/ruby@3.1/bin:$PATH"' >> ~/.zprofile
echo 'export LANG=en_US.UTF-8' >> ~/.zprofile
echo 'export LC_ALL=en_US.UTF-8' >> ~/.zprofile
source ~/.zprofile
```

The `LANG` / `LC_ALL` lines prevent an SCSS encoding error you'd otherwise hit when Jekyll tries to compile theme stylesheets containing non-ASCII characters.

Verify:
```bash
ruby --version
which ruby
```

Expected: `ruby 3.1.x` from `/opt/homebrew/opt/ruby@3.1/bin/ruby`. If `which ruby` still points to `/usr/bin/ruby`, open a brand-new terminal — sometimes the old PATH lingers.

## Step 4 — Project-local gems

Inside your blog's repo:

```bash
cd ~/path/to/your-blog
bundle config set path vendor/bundle
```

This tells Bundler to install gems into `./vendor/bundle/` instead of system-wide. Same idea as Python's `venv` or Node's `node_modules` — **isolation per project**. Two blogs can have different Jekyll versions without conflict, and you can nuke `vendor/` to start fresh anytime.

Make sure these are in your `.gitignore`:

```
vendor/
.bundle/
Gemfile.lock
_site/
.jekyll-cache/
```

(They're huge, platform-specific, and reproducible from `Gemfile` — so they should never be committed.)

## Step 5 — Patch the Gemfile for compatibility

Open your `Gemfile` and add these lines outside the `:jekyll_plugins` group:

```ruby
# Stdlib gems removed from default gems in Ruby 3.x.
# Jekyll 3.9 (pinned by github-pages) still expects them.
gem "csv"
gem "base64"
gem "bigdecimal"
gem "logger"
gem "ostruct"
gem "fiddle"
gem "mutex_m"
gem "webrick"  # local web server, removed from stdlib in Ruby 3.0
```

> 💡 **Why all of these?** Each used to ship with Ruby itself. Ruby 3.4 promoted them to bundled gems that need explicit declaration. Adding them is harmless on GitHub Pages (which runs an older Ruby that has them built-in) and required locally.

**If your Gemfile lists `jekyll-algolia` and you don't actually use Algolia search**, remove it. It pulls in heavy dependencies that break on newer Ruby — and if you're not configuring Algolia in `_config.yml`, you're paying the cost for nothing.

Then install everything:

```bash
bundle install
```

This fetches ~100 gems into `./vendor/bundle/`. Takes 1–3 minutes the first time. Subsequent runs are instant.

## Step 6 — Run the server

```bash
bundle exec jekyll serve --livereload
```

You should see:

```
Server address:     http://127.0.0.1:4000/
LiveReload address: http://127.0.0.1:35729
Server running... press ctrl-c to stop.
```

Open `http://127.0.0.1:4000/` in your browser. Edit any post or page, save — the browser auto-refreshes within a second.

## What `--livereload` actually does

When you save a file:

1. Jekyll's file watcher detects the change
2. Rebuilds the affected pages
3. Sends a notification over a WebSocket connection (port 35729) to your browser
4. A small JS snippet Jekyll injected into your pages tells the browser to refresh

That's the whole "magic." File watcher + WebSocket + a one-line JS injection.

**One caveat:** changes to `_config.yml` need a manual server restart (Ctrl+C, re-run). The config shapes the entire build pipeline, so it's read once at startup. Everything else hot-reloads.

## Daily workflow (after one-time setup)

```bash
cd ~/path/to/your-blog
bundle exec jekyll serve --livereload
```

Edit, save, see changes instantly. `Ctrl+C` to stop.

That's it. No more push-and-pray.

## Pitfalls I hit (so you don't have to)

| Symptom | Cause | Fix |
|---|---|---|
| `command not found: jekyll` | Bundler installed but Jekyll never installed | `bundle install` |
| `ffi requires ruby version >= 3.0` | Apple's Ruby 2.6 is too old | Install Ruby 3.1 via brew |
| `undefined method 'tainted?' for nil` | Ruby 3.2+ removed `tainted?`, Liquid still calls it | Switch to Ruby 3.1 |
| `cannot load such file -- csv` (or `ostruct`, `webrick`...) | Ruby 3.4+ removed stdlib modules | Add the gem to your Gemfile |
| `Invalid US-ASCII character "\xE2"` | Locale not set to UTF-8 | `export LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8` |
| Server starts then immediately crashes | Usually a missing gem | Read the actual error — Ruby errors are unusually specific |

## The lesson worth keeping

The setup tax was real — it took me an hour the first time, mostly because I tried to use the latest Ruby. **Don't skip the boring step of pinning to Ruby 3.1.**

But once it works, every future edit becomes a 1-second feedback loop. That's compounding interest on your setup time. Over the lifetime of a blog, the savings are measured in hours.

> **Slow is fast.** Spend 30 minutes on the setup once. Save it forever.

## What's next

- Try `jekyll build --incremental` for faster rebuilds on large sites once you cross ~50 posts.
- The [Jekyll docs](https://jekyllrb.com/docs/) are genuinely excellent — bookmark them.
- If you ever want to upgrade beyond GitHub Pages' frozen Jekyll 3.9 (e.g., for newer plugins), that's a bigger migration but worth knowing about: drop the `github-pages` gem and pin `jekyll` directly in your Gemfile.

The browser tab at `localhost:4000` is now your real-time blog preview. Welcome to a faster writing loop.
