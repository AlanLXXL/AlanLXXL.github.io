---
title: "Getting Started with Claude (and Claude Code)"
excerpt: "From your first prompt on claude.ai to running Claude Code in a real project — a practical first-week guide for students."
app: "Claude / Claude Code"
difficulty: "Beginner"
time: "30 minutes"
last_tested: 2026-05-05
tags:
  - AI
  - Productivity
  - CLI
---

If your CS curriculum hasn't taught you how to work with an AI assistant yet, you're not alone — but it's quickly becoming as fundamental as Git. This guide is the walkthrough I wish I'd had: how to actually start using Claude in a way that helps you *learn*, rather than just hands you answers.

## Why Claude

Claude is the AI assistant made by Anthropic. The reason I personally lean toward it (vs. other options) is that it tends to write more carefully, ask clarifying questions when things are ambiguous, and is genuinely good at long-form reasoning over real codebases.

You'll meet Claude through several front doors. Pick the one that matches what you're trying to do.

| Where | Best for |
|---|---|
| **claude.ai** (web) | Quick questions, brainstorming, writing, summarizing |
| **Desktop app** (Mac/Windows) | Same as web, friendlier with local files |
| **Claude Code** (CLI) | Working inside a real codebase — reading and editing your project files |
| **API / SDK** | Building apps that use Claude programmatically |

For coursework, **claude.ai** covers most of what you need. For projects with multiple files, **Claude Code** is the game-changer.

## Step 1 — Your first conversation on claude.ai

1. Go to [claude.ai](https://claude.ai) and sign up — your university email may unlock student perks.
2. In the chat box, try a real task. Not "say hello." Something like:

> "I'm a 2nd-year CS student. I just learned about hash tables. Give me an exercise that's slightly harder than what's in a textbook, then walk me through the solution after I try."

This kind of prompt does two useful things: (1) it gives Claude context about *who* you are, and (2) it sets the *interaction style* you want. The more specific you are about both, the better Claude's response.

> 💡 **Slow is fast.** Don't paste your assignment and ask "do this." Ask Claude to explain a concept, give you a smaller problem, then check your work. You'll actually learn — and you won't fail your exam.

## Step 2 — Installing Claude Code

Claude Code is the CLI version that can read and edit files in a project. This is where it becomes genuinely powerful for CS work.

```bash
# Requires Node.js 18+
npm install -g @anthropic-ai/claude-code
```

Then, inside any project directory:

```bash
cd my-project
claude
```

The first run walks you through authentication. After that, you'll see a prompt where you can type instructions in plain English.

## Step 3 — Your first Claude Code session

Try this in a small project — a class assignment is perfect:

> "Look at this codebase and tell me what it does, in plain English."

Claude will read your files and reply with a summary. **Read its response carefully** — it's a great way to spot whether your code does what you *think* it does.

Now try something a little more involved:

> "There's a bug in `solver.py` — it returns `None` for inputs of length 1. Find and fix it."

Claude will:

1. Read the file
2. Show you the proposed change
3. Wait for your confirmation before editing

This confirm-before-edit loop is the most important habit to build. **Always read what it's about to do before approving.**

## Step 4 — Slash commands worth knowing

Inside a Claude Code session, type `/` to see available commands. The ones you'll actually use:

- `/help` — list everything available
- `/clear` — start a fresh conversation (use this when switching tasks; it keeps Claude focused)
- `/init` — generate a `CLAUDE.md` file describing your project
- `/config` — change settings like the active model
- `/cost` — see token usage for the current session

## Step 5 — Leaving notes for future sessions (CLAUDE.md)

Claude doesn't remember between sessions. But if you put a `CLAUDE.md` file at your project root, Claude reads it at the start of every session. Use it for things like:

- "This is a Python project using Flask, please use type hints"
- "The main entry point is `app/main.py`"
- "Tests live in `tests/`, run them with `pytest`"
- "Don't add comments unless I ask"

Run `/init` and Claude will draft one for you based on the project.

## Five ways students actually use Claude

1. **Concept explainer.** "Explain CRDTs like I'm in distributed systems class but haven't seen the lecture yet."
2. **Debugger.** Paste an error message *and* the relevant code. Ask "what's wrong and why."
3. **Code reviewer.** Before submitting an assignment: "review my approach in `solution.py` — is it correct, and is there a cleaner way?"
4. **Rubber duck.** Talk through your approach *before* writing any code. The act of explaining out loud catches half your bugs.
5. **Writing partner.** Drafts of project reports, README files, lab write-ups.

## Pitfalls I've hit (so you don't have to)

- **Don't paste-and-pray.** Copying Claude's code without understanding it leads to bugs you can't fix when the demo breaks.
- **Don't skip the question step.** When Claude asks a clarifying question, answer it — don't just say "do whatever." The clarification is where the real thinking happens.
- **Don't trust it on facts you can verify.** Library versions, API endpoints, exact syntax — double-check.
- **Don't use it as your only tutor.** Claude is great for filling gaps, but you still need to read papers, sit in lectures, and write code yourself.

## What's next

- Read the [Claude Code docs](https://docs.claude.com/en/docs/claude-code) for advanced features (hooks, MCP servers, custom slash commands).
- Try the official **VS Code** or **JetBrains** extensions to use Claude inside your editor.
- Once you're comfortable, look into building something small with the [Claude API](https://docs.claude.com/en/api).

The single biggest mindset shift: stop thinking of Claude as a search engine that returns code. Think of it as a patient, knowledgeable collaborator — most useful when you bring real problems and your own thinking to the table.
