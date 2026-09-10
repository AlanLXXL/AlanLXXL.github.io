---
# ─────────────────────────────────────────────────────────────────────────────
# Copy this file to _projects/<slug>.md and fill it in.
# The slug becomes the URL:  _projects/smart-sensor.md  →  /projects/smart-sensor/
# Remove the `published: false` line (or set it to true) to make it appear.
# See _projects/realtime-chat.md for a complete, filled-in example.
# ─────────────────────────────────────────────────────────────────────────────
published: false

title: "Project Name"
order: 1                              # 1 = shown first on /projects/ and on the homepage
period: "Jan – May 2026"              # when you worked on it
role: "ML engineer · team of 3"       # your role and team size, or "Solo project"
excerpt: "One sentence: what it does and why it matters. Shown on the card."
result: "One concrete outcome, with a number if you have one — e.g. cut inference latency by 40%, 92% accuracy on X, used by N people."
stack:                                # tech stack, shown as tags
  - Python
  - PyTorch
  - FastAPI
links:                                # keep only the ones you have
  - label: "GitHub"
    url: "https://github.com/AlanLXXL/repo-name"
  - label: "Demo"
    url: "https://example.com"
  - label: "Report"
    url: "/assets/files/report.pdf"
# image: /assets/images/projects/project-name.png   # optional thumbnail for the card

# Optional "At a glance" block, rendered above the body on the project page.
# stats: up to four big numbers (tiles). Delete the block to hide it.
stats:
  - value: "92%"
    label: "accuracy"
    note: "on the held-out set"        # optional small print
  - value: "40%"
    label: "lower latency"
    note: "vs. the baseline"
# highlights: two or three cards, one per headline achievement. Markdown works in text.
highlights:
  - title: "Headline achievement one"
    text: "Two sentences: what you did and the concrete outcome."
  - title: "Headline achievement two"
    text: "Two sentences: what you did and the concrete outcome."
---

## Problem

What was the problem, and why did it matter? Two or three sentences.

## What I built

The approach, the architecture, and the decisions you made. This is the part an interviewer will read most closely.

Reusable building blocks (styles live in _includes/head/custom.html):
- an architecture diagram: `<div class="arch">` with `.arch__layer` / `.arch__boxes` / `.arch__box` / `.arch__flow`
- a pipeline strip: `<ol class="pipeline">` with `.pipeline__step` items
- a ranking dot plot: `<div class="dotplot">` with `.dotplot__row` / `.dotplot__track` / `.dotplot__dot`; add `dotplot--dual` for two dots per row (filled + hollow) with a `.dotplot__head` legend
- a captioned figure: `<figure>` + `<img>` + `<figcaption>` (styled by the theme); put images in assets/images/projects/<slug>/ and PDFs in assets/files/
See _projects/realtime-chat.md (arch, pipeline) and _projects/learning-guided-search.md (dotplot, figure) for the markup.

## Results

Numbers, screenshots, or a short demo. Be concrete.

## What I learned

One or two honest takeaways.
