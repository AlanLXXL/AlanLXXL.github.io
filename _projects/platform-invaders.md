---
title: "Platform Invaders: Cross-Platform Space Invaders in C++"
order: 3
period: "Sep 2024 – May 2025"
role: "Game Layer lead · team of 7 · sponsored by Feral Interactive"
excerpt: "Space Invaders rebuilt in C++ on a 2D game library our team wrote from scratch over native macOS and Windows APIs, with Unity, Unreal, and SDL off limits. I led the Game Layer: the entity architecture, the fixed-framerate game loop, and the collision system."
result: "Won Sponsor's Choice at the SWE Demo Day, judged by industry sponsors including Unity and Microsoft; three games (Space Invaders, Snake, Pong) run natively on macOS and Windows from one codebase each."
stack:
  - C++17
  - Object-oriented design
  - Game loop & frame timing
  - AABB collision detection
  - Cocoa / AppKit
  - Direct2D
  - XAudio2 / AVFoundation
  - Xcode
  - Visual Studio
  - GitLab
  - Scrum
image: /assets/images/projects/platform-invaders-game.png
image_alt: "Space Invaders running natively on macOS: five rows of eleven enemies, four shields, the player's cannon, and a score and lives HUD"
stats:
  - value: "3"
    label: "games on the library"
    note: "Space Invaders, Snake, Pong; macOS and Windows"
  - value: "70+"
    label: "demo-day players"
    note: "posted scores to the companion site's live leaderboard"
  - value: "45"
    label: "game requirements met"
    note: "traced item by item in the final report"
  - value: "0"
    label: "third-party engines"
    note: "native Cocoa and Direct2D only; no Unity, Unreal, or SDL"
highlights:
  - title: "One Entity interface, five managers"
    text: "Every visible object implements `update()` and `render()`; enemy, bullet, shield, score, and lives managers each expose a single `updateAndRender()`. The controller stays a thin loop, and the same loop, menu, and leaderboard structure carried into Snake and Pong."
  - title: "Fixed-framerate game loop"
    text: "Each frame drains the input buffer, updates and renders every entity, fires enemy bullets, resolves collisions, then sleeps to the target frame rate. Every speed is a per-frame constant tuned to the arcade original, and identical on both operating systems."
  - title: "Collision detection in one place"
    text: "Axis-aligned rectangle tests live in the `BulletManager`, which only checks pairs that can actually collide. A wave of 55 enemies, four shields, and every bullet in flight is resolved each frame without a broad phase."
gallery:
  - url: /assets/images/projects/platform-invaders-menu.png
    image_path: /assets/images/projects/platform-invaders-menu.png
    alt: "Main menu with arrow-key selection and the points table"
    title: "Main menu: arrow keys select, Enter confirms"
  - url: /assets/images/projects/platform-invaders-leaderboard.png
    image_path: /assets/images/projects/platform-invaders-leaderboard.png
    alt: "Leaderboard listing the top five names and scores"
    title: "Leaderboard: the top five, read back from scores.txt"
  - url: /assets/images/projects/platform-invaders-gameover.png
    image_path: /assets/images/projects/platform-invaders-gameover.png
    alt: "Game over screen with name entry and the final score"
    title: "Game over: type a name to save the score"
---

## Problem

Feral Interactive ports AAA games from Windows to other platforms, and its brief for our University of Nottingham industry project was built around that work: remake Space Invaders in C++ so that it runs on both Windows and macOS, using each operating system's own APIs. Unity, Unreal, SDL, and anything like them were explicitly disallowed, because the point was to learn the abstraction techniques that porting depends on.

That made two products out of one brief. The library, an abstraction layer over the OS APIs, was the primary deliverable and had to stay game-agnostic. The game on top of it had to be a faithful Space Invaders and had to prove the library worked. On a team of seven, I led the Game Layer sub-team through both milestones, the proof of concept in December 2024 and the final product in May 2025, and I was also the team administrator: the document repository, the preliminary report, and the final submission on deadline day.

{% include figure image_path="/assets/images/projects/platform-invaders-game.png" alt="Space Invaders running natively on macOS: five rows of eleven enemies, a red mystery enemy, four shields, the player's cannon, and a score and lives HUD" caption="The finished game on macOS. Every sprite, digit, and letter is an image drawn through the library's renderer interface; the same Game Layer builds unchanged for Windows." %}

## Architecture

<div class="arch">
  <div class="arch__layer arch__layer--accent">
    <div class="arch__layer-label">Game Layer &middot; pure C++, no OS headers &middot; my part</div>
    <div class="arch__boxes">
      <div class="arch__box"><strong>GameController</strong><span>creates the window, sets the target frame rate, spawns the wave, runs the loop</span></div>
      <div class="arch__box"><strong>Entity interface</strong><span><code>update()</code> and <code>render()</code> every frame: Player, Enemy, MysteryEnemy, Bullet, Shield</span></div>
      <div class="arch__box"><strong>Managers</strong><span>Enemy, Bullet, Shield, Score, Lives: one <code>updateAndRender()</code> each; collisions live in BulletManager</span></div>
      <div class="arch__box"><strong>GUI controller</strong><span>a state machine over the Main Menu, Leaderboard, and Game Over screens</span></div>
    </div>
  </div>
  <div class="arch__flow">
    <span class="arch__flow-item"><code>platformInterface-&gt;getVisualRenderer()-&gt;renderImage(image, x, y)</code> &darr;</span>
  </div>
  <div class="arch__layer">
    <div class="arch__layer-label">Abstraction Layer &middot; five interfaces plus Image and Audio objects</div>
    <div class="arch__boxes">
      <div class="arch__box"><strong>VisualRenderer</strong><span>create the window; render, move, remove images; <code>updateDisplay()</code> flushes the frame</span></div>
      <div class="arch__box"><strong>ClockManager</strong><span><code>setTargetFrameRate()</code>, <code>synchronizeFrame()</code>, timestamps</span></div>
      <div class="arch__box"><strong>UserInputManager</strong><span>key events buffered into a FIFO of <code>UserInput</code> enums</span></div>
      <div class="arch__box"><strong>AudioRenderer</strong><span>play and stop .wav sounds</span></div>
      <div class="arch__box"><strong>ResourceManager</strong><span>builds Image and Audio objects from asset names; reads and writes text files</span></div>
    </div>
  </div>
  <div class="arch__flow">
    <span class="arch__flow-item">conditional compilation picks the OS objects once, at start-up &darr;</span>
  </div>
  <div class="arch__layer">
    <div class="arch__layer-label">OS-Specific Layer &middot; one implementation per interface per platform</div>
    <div class="arch__boxes">
      <div class="arch__box"><strong>macOS</strong><span>Cocoa/AppKit window and sprite views, AVFoundation audio, <code>mach_absolute_time</code>, NSBundle asset paths</span></div>
      <div class="arch__box"><strong>Windows</strong><span>Direct2D and WIC rendering, XAudio2 audio, <code>QueryPerformanceCounter</code>, Win32 message loop</span></div>
    </div>
  </div>
</div>

The rule that shaped my side of the design: the Game Layer includes exactly one library header, `PlatformInterface`, and everything it draws, plays, times, or reads goes through those five interfaces. Nothing in the game knows which OS it is on. The team split the same way: three of us on the Game Layer, a Windows sub-team, a macOS sub-team, and single owners for the clock and audio systems.

The boundary was not right the first time. In the December proof of concept the renderer took a whole list of game entities each frame, so the library depended on a Game Layer type, and Feral flagged the coupling in their review. Over January and February the redesign replaced that with `Image` and `Audio` objects owned by the library and incremental render, move, and remove calls that the game makes per entity. I worked on the redesign from the game's side of the boundary, pinning down what a game loop actually needs from a renderer, and maintained the class diagram that specified it. It is the same lesson as the database interface in [my chat project](/projects/realtime-chat/): the interface is the contract, so define it from the caller's needs.

## What I built

### The game loop

In outline (not verbatim), the loop the controller runs:

```cpp
renderer->createWindow(WIN_WIDTH, WIN_HEIGHT, "Space Invaders", false);
clock->setTargetFrameRate(FPS);
spawnWave();                                   // 5 x 11 enemies, 4 shields, player, HUD

while (state == Running) {
    while (!input->isEmpty())                  // drain every buffered key event
        processInput(input->getNextInput());   // A / D move, the fire key shoots
    for (GameManagerInterface* m : managers)
        m->updateAndRender();                  // enemies, bullets, shields, score, lives
    enemyManager->maybeFire();                 // random intervals, about 1/s rising to 2/s
    bulletManager->checkCollisions(...);       // rectangle tests, see below
    if (enemyManager->lowestY() >= SHIELD_LINE) state = GameOver;
    renderer->updateDisplay();                 // flush buffered draws, pump OS events
    clock->synchronizeFrame();                 // sleep away the rest of the frame
}
```

Three decisions worth explaining:

- **Drain the input buffer, do not sample it.** The library buffers key-down and key-up events; the loop consumes all of them every frame. Handling one event per frame would space a burst of key presses a whole frame apart, about 33 ms at 30 fps, which is a visible lag when someone mashes fire.
- **Fixed frame rate over a variable timestep.** `synchronizeFrame()` measures the time since the last frame and sleeps the remainder, so every speed in the game is a per-frame constant tuned once against the arcade timings: about three seconds for the player to cross the screen, two for a player bullet to climb it, three and a half for an enemy bullet to fall. Because the clock sits behind the interface (Mach absolute time on macOS, the performance counter on Windows), the tuning holds on both platforms. The trade-off is that a machine too slow for the frame budget slows the game rather than dropping frames; we accepted that for a 2D arcade game.
- **One `updateDisplay()` per frame.** Draw calls are buffered by the library and processed together, and that call is also when the OS event loop is pumped (a non-blocking `PeekMessage` loop on Windows, `nextEventMatchingMask` on macOS). Calling it more often flickers animations; not calling it freezes both the picture and the keyboard.

### Entities, managers, and animation

Every visible object implements an `Entity` interface with `update()` and `render()`. `update()` changes state: position, health, which animation image should be showing. `render()` reconciles the screen with that state. For plain movers that is one `moveRenderedImage` call. Animated entities (the marching enemies, the player and enemies dying, bullets) hold a list of images, a frame threshold, the image currently rendered, and the image that should be rendered, and their `render()` is a diff:

```cpp
// if what's currently rendered isn't what it should be, then update it
if (currentRenderedImage != getImage()) {
    renderer->removeImage(currentRenderedImage);
    renderer->renderImage(getImage(), getPosition().x, getPosition().y);
    currentRenderedImage = getImage();
} else {
    renderer->moveRenderedImage(getImage(), getPosition().x, getPosition().y);
}
```

The library has no sprite-sheet or animation support, so this diff is the whole animation system, and it only touches the renderer when something changed.

Above the entities sit managers, each implementing a `GameManagerInterface` with a single `updateAndRender()`:

- **EnemyManager** owns the 5 × 11 formation. It marches sideways, reverses and steps down at the window edge, speeds up as enemies die, and spawns a faster wave when the last one falls. The mystery enemy appears at random intervals, crosses the top of the screen, and is worth 50 to 300 points.
- **BulletManager** spawns player and enemy bullets, moves them, culls the ones that leave the window, and owns collision detection.
- **ShieldManager** keeps the four shields, swapping sprites as they take damage from either side and removing them at zero health.
- **ScoreManager** and **LivesManager** draw the HUD from digit images, because the library has no text rendering, and each re-renders only when its value changes.

The controller is therefore a thin loop over managers. Feral's Easter review asked for more polymorphism and a more modular controller, and the manager split and the GUI state machine below are what the last two sprints delivered in response.

### Collision detection

Collision is not a library feature, so it lives in the game. Two entities collide when the rectangles formed by their positions and image sizes intersect: an axis-aligned bounding-box test. The checks sit in `BulletManager` behind an overloaded `checkCollision()` for three reasons:

- it keeps the controller readable: one call per frame, with the outcomes handled in one place;
- it only tests pairs that can collide: player bullets against enemies and shields, enemy bullets against the player and shields, never a bullet against its own shooter;
- it is one-phase by design. With at most 55 enemies, four shields, and a handful of bullets in flight, a broad phase would cost more than it saves; the report lists a two-phase approach as a future extension.

The outcomes: an enemy bullet kills the player, who plays a two-stage death animation, loses a life, and respawns after a delay if any lives remain; a player bullet kills an enemy, scores points that depend on its row, and plays its explosion; either kind of bullet damages a shield. One collision is not a rectangle test at all: if the lowest enemy reaches the shield line, the game ends. That is a single minimum-Y comparison in the controller, so it never touches the bullet system.

### Menus, leaderboard, and game over

Around the game sits a small state machine. A controller loops over a `GUIState` enum, and each screen implements `GUIInterface::loop()`, runs until the player leaves it, and returns the next state. The main menu is arrow-key selection. The leaderboard reads `scores.txt` through the library's file interface, parses name and score pairs, sorts them, and renders the top five. Game over lets the player type a name, drawing each character as it arrives, and writes the score back. All of the text is images, because the library has none.

{% include gallery caption="The three screens around the game, all drawn from image assets through the same renderer interface." %}

Sound follows the same one-call pattern, with one idea I like: the original's four-note bass line is four separate `.wav` files cycled in step with the enemy march, so the music speeds up exactly as the enemies do. One long track could never have followed the tempo.

### Testing

We followed test-driven development on the Game Layer with a small in-house unit-test framework; an open-source one was judged too costly to set up before the proof of concept. The `GameController` tests ran against a mocked platform interface, so they needed no window: polling the input buffer with and without pending input, processing left and right movement, starting the game, one pass of the loop, and a single-step run. The reports show the pattern: all seven failing on 4 December 2024 before the code existed, all seven passing the next day. Visual and audio behaviour was verified with QA-style checklists per interface, one report for each OS.

An honest limit: automated coverage stayed thin after the proof of concept. Weekly working demos to the sponsors and peer-reviewed merges carried more of the verification than the test suite did, and the final report says so.

## Results

- **Sponsor's Choice at the SWE Demo Day**, judged by industry sponsors including Unity and Microsoft.
- **Three games from one library.** Space Invaders was the target; teammates then built Snake and Pong on the same interfaces, and Snake's menu and leaderboard mirror the Space Invaders code. All three run natively on macOS and Windows.
- **Fewer lines than SDL for the same job.** Opening a window, drawing a sprite, and moving it with the keyboard took 34 lines on our library against 45 with SDL, and in a 30-second macOS test the library used less CPU than SDL for a static and a moving sprite. It also does far less than SDL, which the report says plainly.
- **Requirements traced.** The final report checks 45 functional game requirements off against the shipped Game Layer, from the 5 × 11 formation to the four-note music.
- **A companion site for demo day.** I also built and deployed a full-stack companion site where more than 70 players posted their scores to a live leaderboard and chatted about the games during the day.
- **Process.** Twelve two-week sprints with story points, sprint plans, and retrospectives; I led the Game Layer task in every sprint from its planning in February 2025 to the tagged submission in May.

The source is on the University of Nottingham's internal GitLab, so there is no public link. I am glad to walk through the game loop, the entity code, or the collision system in conversation.

## What I learned

- **Define the interface from the caller's side.** The proof of concept leaked a game type into the renderer; the fix was to make the library own `Image` and `Audio` objects and take incremental calls. Every interface I have designed since starts from what the loop needs, not from what the platform offers.
- **Fix the timestep before tuning anything.** Until `synchronizeFrame()` existed, speeds were tuned per machine; afterwards every speed was one constant, matched to the arcade game once and correct on both platforms.
- **Ship increments and escalate early.** We lost weeks in the first semester to a blocked OS event loop. In the second semester the Game Layer showed a working increment every week, and any bug that beat its owner went to another teammate with a written diagnosis attached.
- **Sponsor feedback is a spec.** Feral's Easter review, asking for polymorphism, new waves, and a score fix, set the backlog for the last two sprints, and answering it is what turned a working game into a clean one.
