---
title: "Chrome for Computer Science Students: DevTools, Extensions, and Setup"
excerpt: "The Chrome features that aren't in any course — DevTools tricks, must-have extensions, and a setup that pays for itself in your first week of debugging."
app: "Google Chrome"
difficulty: "Beginner"
time: "45 minutes"
last_tested: 2026-05-05
tags:
  - Chrome
  - Web Development
  - Productivity
---

Most CS programs assume you "know how to use a browser." But there's a difference between *using* Chrome and *using it like a developer*. This guide covers the parts of Chrome you'll actually need — the kind of things senior students take for granted but no one ever explicitly teaches.

## Why Chrome (specifically)

Firefox and Safari are good. But Chrome's developer tools are the most widely-taught and well-documented, and most web tutorials assume you're using them. Pick another browser later if you want — but learn the dev workflow on Chrome first, because that's where the documentation lives.

## Part 1 — DevTools, the tour you should have gotten on day one

Open DevTools with **Cmd+Option+I** (Mac) or **F12** (Windows/Linux). Or right-click any element on a page and choose *Inspect*.

You'll see tabs across the top. Here's what each one is actually for:

### Elements
The live HTML and CSS of the page. You can edit either right there and see changes instantly — *they don't persist after refresh*, but that's exactly what makes it perfect for trying things out.

> 💡 Try this: open any website, hit Inspect, and change a heading's text. The change appears on screen but disappears on refresh. This is how every front-end developer prototypes visual changes.

### Console
Run JavaScript against the current page. Type `document.title` and hit Enter. Try `document.querySelectorAll('a').length` to count every link. The console is also where `console.log()` output from any web app shows up — including yours.

### Sources
Where the JavaScript files live. You can set **breakpoints** by clicking in the gutter next to a line number, and the page will pause there next time it runs that code. This is real debugging — much better than `console.log` everywhere.

### Network
Every request the page makes — HTML, CSS, JS, images, API calls. Filter by **Fetch/XHR** to see only API traffic. This is how you'll inspect REST and GraphQL calls when building any web app.

### Application
Local storage, session storage, cookies, IndexedDB. When your login isn't working, this is where you check whether the auth token actually got saved.

### Lighthouse
Generates a one-click report on performance, accessibility, SEO, and best practices for any page. Run it on any major website to see how a real audit looks.

### Performance & Memory
For when you've shipped something and it's slow. You probably won't need these in your first year, but know they're there.

## Part 2 — Keyboard shortcuts that actually save time

| Shortcut (Mac / Win) | What it does |
|---|---|
| **Cmd+T / Ctrl+T** | New tab |
| **Cmd+W / Ctrl+W** | Close current tab |
| **Cmd+Shift+T / Ctrl+Shift+T** | Reopen the tab you just closed (the one you'll use most) |
| **Cmd+L / Ctrl+L** | Jump to address bar |
| **Cmd+1..8 / Ctrl+1..8** | Jump to a specific tab |
| **Cmd+Option+I / F12** | Open DevTools |
| **Cmd+Shift+P / Ctrl+Shift+P** (in DevTools) | Run any DevTools command — like a Spotlight for the browser |

The last one is a hidden gem. Open DevTools, hit it, type "screenshot," and you can take a full-page screenshot — no extension needed.

## Part 3 — Extensions worth installing

Be picky. Every extension has access to your pages — only install what you trust and actually use.

**For everyone:**
- **uBlock Origin** — fast, open-source ad and tracker blocker. Makes browsing faster and quieter.
- **Bitwarden** or **1Password** — a password manager. Stop reusing passwords; this is non-negotiable.

**For web development:**
- **React Developer Tools** — inspect React component trees and state. Any frontend course will need this.
- **Vue.js devtools** — same, for Vue.
- **Redux DevTools** — time-travel debugging for Redux state.
- **JSON Viewer** — makes raw JSON responses readable instead of a wall of text.
- **Wappalyzer** — tells you what tech stack any website uses (frameworks, analytics, hosting). Great for learning by reverse-engineering.

**For productivity:**
- **Vimium** — Vim-style keyboard navigation. Steep learning curve, big payoff.
- **OneTab** — collapse 30 open tabs into one list. Saves memory and your sanity.

## Part 4 — Profiles (the underrated feature)

Click your profile picture in the top-right corner → *Add* → create a separate profile.

Why bother? Because you probably want to keep separated:

- **Personal** — Gmail, social, shopping
- **University** — coursework, research, your university Google account
- **Development** — clean profile, no extensions interfering when debugging

Each profile has its own bookmarks, history, extensions, and logged-in accounts. Switching is one click.

## Part 5 — DevTools Overrides (editing live websites)

Want to test a CSS change against production without redeploying? In DevTools, go to **Sources** → **Overrides** → choose a folder. Now any file you edit in DevTools gets saved to that folder and persists across refreshes — Chrome serves your edited version instead of the live one.

This is incredibly useful for:

- Testing a fix before opening a PR
- Reproducing a bug locally
- Showing a designer a CSS change without a build step

## Part 6 — Lighthouse for course projects

Before submitting any web project, run Lighthouse on it. Open DevTools → Lighthouse → Analyze. You'll get scored on:

- **Performance** — how fast it loads
- **Accessibility** — can a screen reader use it
- **Best Practices** — HTTPS, modern image formats
- **SEO** — are you tagging things correctly

A 90+ score on a class project tells your tutor you actually care. A 30 tells them you didn't.

## Pitfalls

- **Don't install 30 extensions.** Each one is another security and performance liability. Audit yours every few months.
- **Don't rely on Inspect-and-edit forever.** The changes don't persist; eventually you have to edit the actual source.
- **Don't forget to clear cache when debugging weird issues.** Right-click the refresh button (with DevTools open) → *Empty Cache and Hard Reload*. Saves you from chasing imaginary bugs.

## What's next

- Read the official [Chrome DevTools docs](https://developer.chrome.com/docs/devtools) — they're excellent.
- Learn one keyboard shortcut a week instead of trying to memorize them all at once.
- Try Firefox DevTools too once you're comfortable — they're great for CSS work specifically.

The browser is the most-used piece of software in your life. Spending two hours learning it properly will pay off every single day for the rest of your career.
