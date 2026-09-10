---
title: "Full-Stack Real-Time Chat Platform"
order: 1
period: "May – Aug 2026"
role: "Solo developer · CMU-SV course project"
excerpt: "A TypeScript chat app with JWT authentication, message reactions pushed live over WebSockets, one persistence interface behind two databases, and a pipeline that lints, builds, tests, and deploys every push to Render."
result: "61 Jest integration tests pass against both the in-memory and MongoDB Atlas backends; 28 reviewed pull requests shipped through the CI/CD pipeline."
stack:
  - TypeScript
  - Node.js
  - Express
  - Socket.IO
  - MongoDB Atlas
  - Mongoose
  - JWT
  - bcrypt
  - Jest
  - GitHub Actions
  - Render
links:
  - label: "Live app"
    url: "https://yaca-xinglu-m26.onrender.com"
stats:
  - value: "61"
    label: "integration tests"
    note: "same Jest suite, both databases"
  - value: "28"
    label: "pull requests"
    note: "reviewed, CI-gated, one feature branch each"
  - value: "2"
    label: "storage backends"
    note: "behind one 13-method interface"
  - value: "0"
    label: "controller changes"
    note: "to swap in-memory for MongoDB Atlas"
highlights:
  - title: "Live reactions over WebSockets"
    text: "A user story I specified myself: react to any message with one of six emoji. The server persists the reaction, then broadcasts a `reactionUpdate` event so every open chat window redraws that message's reaction bar without a refresh."
  - title: "One DAO interface, two databases"
    text: "Controllers and models only ever talk to a single `IDatabase` interface. An in-memory adapter powered early development; the MongoDB Atlas adapter dropped in behind the same interface with no change to API controllers, and the same 61 tests passed on both."
  - title: "CI/CD on every push to main"
    text: "GitHub Actions lints, builds, and runs the 61 Jest REST-integration tests on every push to main, then triggers a Render deploy hook. All 28 pull requests went through that gate."
---

## Problem

The brief for the CMU-SV full-stack development course (18-651) was YACA, "Yet Another Chat App": a chat room with registration, login, and a friend list, grown from a thin TypeScript starter. Everyone ships the same required user stories, so the interesting part was the bar I set for my own build: typed end to end, tested against a real database rather than only an in-memory one, deployed automatically instead of by hand, and extended with a feature I specified and designed myself: message reactions that appear live for everyone in the room.

## Architecture

<div class="arch">
  <div class="arch__layer">
    <div class="arch__layer-label">Browser &middot; TypeScript client, bundled by Parcel</div>
    <div class="arch__boxes">
      <div class="arch__box"><strong>Pages</strong><span>HTML and Pug views: home, auth, friends, chat</span></div>
      <div class="arch__box"><strong>Axios REST calls</strong><span>bearer JWT on every protected request</span></div>
      <div class="arch__box"><strong>socket.io-client</strong><span>listens for <code>newChatMessage</code> and <code>reactionUpdate</code></span></div>
    </div>
  </div>
  <div class="arch__flow">
    <span class="arch__flow-item">HTTP + JSON &darr;</span>
    <span class="arch__flow-item">&uarr; WebSocket push to every connected client</span>
  </div>
  <div class="arch__layer arch__layer--accent">
    <div class="arch__layer-label">Node.js server &middot; Express and Socket.IO sharing one port</div>
    <div class="arch__boxes">
      <div class="arch__box"><strong>Controllers</strong><span><code>/auth</code>, <code>/chat</code>, <code>/friends</code>: routing, JWT middleware, status codes</span></div>
      <div class="arch__box"><strong>Models</strong><span>User, ChatMessage, Reaction: validation, bcrypt, UUIDs, business rules</span></div>
      <div class="arch__box"><strong>Socket.IO server</strong><span>injected into the controller base class; emits after each successful write</span></div>
    </div>
  </div>
  <div class="arch__flow">
    <span class="arch__flow-item"><code>IDatabase</code> interface &middot; 13 async methods &darr;</span>
  </div>
  <div class="arch__layer">
    <div class="arch__layer-label">Persistence &middot; chosen by the STAGE variable at startup</div>
    <div class="arch__boxes">
      <div class="arch__box"><strong>InMemoryDB</strong><span>early development and local test runs; deep-copies on every read and write</span></div>
      <div class="arch__box"><strong>MongoDB via Mongoose</strong><span>DEV and PROD databases on Atlas; schemas mirror the shared interfaces; wipe guard on PROD</span></div>
    </div>
  </div>
</div>

Two decisions shape everything above. A `common/` folder of TypeScript interfaces is imported by both client and server, so socket event names and payloads are compile-checked on both ends and the emoji list exists in exactly one place. And both the database and the Socket.IO server are injected into base classes as static dependencies, which is what makes each of them swappable without touching the code that uses them.

## What I built

### Real-time message reactions

This was the user story I specified myself, from acceptance criteria to REST design. Reactions are a nested sub-resource of a message:

```
POST   /chat/messages/:messageId/reactions                add a reaction
GET    /chat/messages/:messageId/reactions                list a message's reactions
DELETE /chat/messages/:messageId/reactions/:reactionId    remove your own reaction
```

The rules I am most careful about:

- **Identity comes from the token.** The reactor's username is taken from the verified JWT, never from the request body, so nobody can react as someone else.
- **One reaction per (message, user, emoji).** Checks run in a deliberate order: request shape first (400), then whether the message exists (404), then duplicates (409), so a bad message id never reports as a conflict.
- **Only the owner can delete.** A foreign delete gets 403, and existence is checked before ownership, so a forbidden request leaves the reaction untouched.
- **The server is the single source of truth.** After a successful add or delete, the controller emits `reactionUpdate` to every connected socket. The client that made the request does not redraw optimistically; it waits for the same broadcast as everyone else, so every open window converges on the identical reaction bar.

### One persistence interface, two databases

Controllers never touch the database. They call models, and models call a single `IDatabase` interface of 13 async methods: connect, init, close, and the user, message, and reaction queries. At startup the STAGE variable picks the implementation: an in-memory store for early development and local test runs, or MongoDB Atlas through Mongoose for the DEV and PROD stages.

The MongoDB adapter is written so that callers cannot tell the two apart:

- Mongoose schemas mirror the shared interfaces field for field, with the version key disabled so no `__v` leaks into payloads.
- Document ids are app-generated UUID strings rather than ObjectIds, so responses are byte-for-byte the same whichever store is behind them.
- Every query uses `lean()` and every save returns `toObject()`, handing callers plain copies. That is the same isolation the in-memory store gets from deep-cloning.
- `init()` wipes every collection, so it throws if the stage is PROD, and startup never requests it there. Two independent guards against wiping production data.

Bringing Atlas online touched the adapter file plus a 13-line change to the startup wiring. No controller or model changed, and the same 61 integration tests passed against both stores.

### Authentication

Registration checks that the username is an email address and that the password contains a letter, a number, and a special character, then stores a bcrypt hash with 10 salt rounds. Login compares against the hash and signs a JWT carrying the username. A small `authorize` middleware guards every message and reaction route: it verifies the bearer token and records the user for the handler downstream. Errors map to explicit status codes: 401 for a missing or invalid token, 400 for validation failures, and 403, 404, and 409 where the reaction spec calls for them.

### Continuous integration and delivery

<ol class="pipeline">
  <li class="pipeline__step"><strong>Push to main</strong><span>a feature branch merged through a pull request</span></li>
  <li class="pipeline__step"><strong>ESLint</strong><span>typed lint across client, server, and tests</span></li>
  <li class="pipeline__step"><strong>Parcel build</strong><span>client bundle and server compile</span></li>
  <li class="pipeline__step"><strong>61 Jest tests</strong><span>REST integration suites, every push</span></li>
  <li class="pipeline__step"><strong>Render deploy hook</strong><span>a second job, after the test job</span></li>
  <li class="pipeline__step pipeline__step--end"><strong>Live</strong><span>yaca-xinglu-m26.onrender.com</span></li>
</ol>

The workflow writes the `.env` from a repository secret, installs with `npm ci`, lints, builds client and server with Parcel, and runs the Jest REST suites. A second job then calls Render's deploy hook. One lesson from getting it live: Render's default Node runtime (26) crashed `jsonwebtoken` on a removed `SlowBuffer` API, and a one-line `.node-version` pin to Node 20 fixed the deployment.

## Try it

<p><a class="btn btn--primary" href="https://yaca-xinglu-m26.onrender.com" target="_blank" rel="noopener">Open the live app</a></p>

1. The first load can take about thirty seconds while Render wakes the free instance up.
2. Choose **Login**, then register with any email-shaped username, a display name, and a password of at least four characters containing a letter, a number, and one of `$ % # @ ! * & ~ ^ - +`.
3. Open **Chat** and post a message. Then open the same URL in a second browser window (a private window works) and register a second user.
4. Click an emoji under a message in one window. The count updates in the other window without a refresh. Click it again to remove the reaction, and watch the count drop everywhere.

The repository is private under the course's academic-integrity policy, so there is no GitHub link. I am glad to walk through any part of the code in conversation.

## Results

| Suite | Tests | What it covers |
|---|---:|---|
| Navigation | 7 | pages and redirects |
| Authentication | 15 | registration rules, login, tokens |
| Chat messages | 14 | posting, listing, authorization |
| Reactions | 25 | add, list, delete; uniqueness, ownership, status codes |
| **Total** | **61** | run on every push, passing on both stores |

- 28 pull requests, each from its own feature branch, merged over the summer.
- The app is live on Render, deployed by the pipeline rather than by hand.
- The reaction feature works across two browsers with no refresh, backed by the persistence layer on Atlas.

## What I learned

- **Program to the interface before you need the second implementation.** The database swap was cheap only because models had never seen a Mongoose document. The interface was the contract, and the test suite proved both sides honoured it.
- **Let the server own real-time state.** Not rendering reactions optimistically cost a few milliseconds of latency and removed an entire class of "my window disagrees with yours" bugs. Every client applies the same event in the same order.
- **Pin your runtime.** A deploy that worked locally and in CI failed on Render because the platform picked a newer Node. Now the version is a file in the repo, not an assumption.
