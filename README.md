# Glitch Runner Sprint

Glitch Runner is a small 2D platformer written in Python with [pygame](https://www.pygame.org/). You run, jump and wall-jump to the exit portal while the game randomly glitches: gravity flips, platforms vanish, your controls lag.

This repo is set up for a **beginner-friendly Python sprint**. You don't need game development experience. If you can read a Python function and run a command in a terminal, you can contribute.

- [Quick Start](#quick-start)
- [How to Play](#how-to-play)
- [How the Sprint Works](#how-the-sprint-works)
- [Sprint Tasks](#sprint-tasks)
- [Project Tour](#project-tour)
- [Troubleshooting](#troubleshooting)

---

## Quick Start

There are two ways to get set up. **Codespaces is the easiest**: nothing to install, it runs in your browser.

### Option A: GitHub Codespaces (recommended)

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/Celeru00/Glitch-Runner-Sprint)

1. Click the button above (or on GitHub: **Code → Codespaces → Create codespace on master**).
2. Wait a few minutes while it sets up. Python, uv and all the libraries are installed for you.
3. In the terminal at the bottom, start the game:
   ```bash
   uv run glitch-runner
   ```
4. Open the **Ports** tab next to the terminal, find **Game Desktop (6080)** and click the globe icon. A new browser tab opens.
5. Click **Connect**. You'll see a desktop with the game window. Click inside the game window before pressing keys.

Leave that tab open. Every time you run the game, it appears there.

> Codespaces has no sound. Everything else works the same.

When you push your first change, GitHub will offer to create a fork for you. Say yes.

### Option B: On your own computer

**1. Install uv**

[uv](https://docs.astral.sh/uv/) installs Python and every library this project needs. You don't need to install Python yourself.

macOS / Linux:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Windows (PowerShell):

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Close and reopen your terminal, then check it worked:

```bash
uv --version
```

**2. Get the code**

Fork this repository on GitHub, then clone your fork:

```bash
git clone https://github.com/<your-username>/Glitch-Runner-Sprint.git
cd Glitch-Runner-Sprint
```

**3. Install and play**

```bash
uv sync
uv run glitch-runner
```

A game window should open. Press **Enter** to start.

### Run the checks

Whichever option you chose:

```bash
uv run pytest           # tests
uv run ruff check .     # lint
uv run ruff format .    # auto-format your code
```

If all three pass, you're ready.

---

## How to Play

| Key         | Action                                 |
| ----------- | -------------------------------------- |
| Left/Right  | Move                                   |
| Space       | Jump (press again in the air to double jump) |
| M           | Mute or unmute music                   |
| Enter       | Start / continue                       |

Slide down a wall by pushing into it while falling, then press Space to wall jump.

- **5 levels**, from "First Glitches" to "System Collapse". Levels 4 and 5 shake constantly, and level 5 adds extra screen corruption.
- **3 enemy types**: basic (patrols), jumper (hops every 2 seconds) and shooter (fires at you when you're level with it).
- **Lives** are set per level: 2 on level 1, up to 8 on level 5. Run out and you're back at the menu.
- **Score**: 1000 points per level, plus a bonus for finishing in under 60 seconds.

### Glitches

Every 10 seconds a random glitch hits for 5 seconds:

| Glitch                | Effect                          |
| --------------------- | ------------------------------- |
| Reversed Gravity      | You fall upwards                |
| Flickering Sprites    | The level blinks in and out     |
| Input Lag             | Your key presses arrive late    |
| Color Distortion      | The screen gets a colour tint   |
| Screen Shake          | The screen shakes               |
| Platform Disappear    | 1 to 3 platforms fade away      |
| Speed Change          | You move faster or slower       |
| Pixelation            | The screen goes blocky          |

---

## How the Sprint Works

1. **Pick a task** from [Sprint Tasks](#sprint-tasks). Start with Level 1 if you're new to Python or to this codebase. Tell an organiser which task you've picked so two people don't do the same one.
2. **Create a branch** for it:
   ```bash
   git checkout -b my-task-name
   ```
3. **Make your change.** Run the game often with `uv run glitch-runner` to see it working.
4. **Check your work:**
   ```bash
   uv run ruff format .
   uv run ruff check .
   uv run pytest
   ```
5. **Commit and push**, then open a pull request against this repo. In the description, say which task you did and how you tested it.

### Ground rules

- One task per pull request. Small PRs get reviewed faster.
- All three checks must pass.
- If you change game rules, add or update a test in `tests/` if you can.
- Stuck for more than 15 minutes? Ask. That's what the sprint is for.

---

## Sprint Tasks

Tasks are grouped by difficulty. Each one lists the files to look at and a hint. The hints point you in the right direction without giving the answer away.

### Level 1: Warm-up

Good first tasks. Mostly changing values and text.

**1.1 Change the feel of the game**
Make the player jump higher, run faster or fall more slowly.
- Files: `src/glitch_runner/config.py`
- Hint: look at `PLAYER_SPEED`, `JUMP_POWER`, `GRAVITY` and `MAX_FALL_SPEED`. Small changes make a big difference.

**1.2 New colour scheme**
Give platforms, enemies or the glitch text new colours.
- Files: `src/glitch_runner/config.py`
- Hint: colours are `(red, green, blue)` tuples from 0 to 255.

**1.3 Put your name on the menu**
Add a "Made by" line to the start screen.
- Files: `src/glitch_runner/presentation/renderer.py`
- Hint: find `MENU_LINES`.

**1.4 Change how often glitches happen**
- Files: `src/glitch_runner/config.py`
- Hint: `GLITCH_INTERVAL` and `GLITCH_DURATION` are in seconds.

**1.5 Add WASD controls**
Let players use A/D to move and W to jump, as well as the arrow keys.
- Files: `src/glitch_runner/presentation/input.py`
- Hint: there are three places to update. Key presses, key releases, and `held_keys()`, which checks which keys are held down every frame.

### Level 2: Core

Add a small feature. You'll read a bit more code.

**2.1 Design level 6**
- Files: `src/glitch_runner/domain/levels.py`
- Hint: copy an existing `LevelSpec`, change the platforms and enemies, then add it to `LEVELS` at the bottom. Platforms are `(x, y, width, height)`. The screen is 800 × 600 and `y` counts down from the top.
- Done when: you can play through it and reach the exit.

**2.2 Add the missing sounds**
The game looks for `land.wav`, `glitch.wav` and `level_complete.wav` but they don't exist yet.
- Files: `src/glitch_runner/assets/sounds/`, `src/glitch_runner/infrastructure/audio.py`
- Hint: `SOUND_FILES` lists the file names the game looks for. Find free sound effects on a site like [Pixabay](https://pixabay.com/sound-effects/) and check the licence.
- Extra: `Sound.LAND` exists but nothing plays it. Make it play when the player lands. Look at how the jump sound is played in `application/session.py`.

**2.3 High score**
Remember the best score and show it on the menu.
- Files: `src/glitch_runner/application/session.py`, `src/glitch_runner/presentation/renderer.py`
- Hint: `GameSession` already tracks `score`. Look at where the score is reset to 0.
- Extra: save it to a file so it survives restarting the game.

**2.4 Time bonus countdown**
Show the remaining time bonus in the HUD while playing.
- Files: `src/glitch_runner/application/session.py`, `src/glitch_runner/presentation/renderer.py`
- Hint: look at `_complete_level` to see how the bonus is worked out.

**2.5 Write tests for enemies**
There are tests for the player, glitches and game flow, but none for enemies.
- Files: create `tests/test_enemy.py`, read `src/glitch_runner/domain/enemy.py`
- Hint: `tests/test_player.py` is a good example. Ideas: an enemy turns around at the end of its patrol, a shooter fires when the player is level with it.

### Level 3: Bug Hunt

These are real bugs in the game. Find out why each one happens and fix it. For 3.2 and 3.3, add a test that proves it's fixed.

**3.1 The level name is shown twice**
The HUD says "Level 1: Level 1: First Glitches".
- Files: `src/glitch_runner/presentation/renderer.py`

**3.2 Wall jumps don't push you away from the wall**
A wall jump should kick you sideways, but you only go straight up.
- Files: `src/glitch_runner/domain/player.py`
- Hint: the wall jump sets `velocity_x`. Look at what happens to `velocity_x` at the start of the very next `update`.

**3.3 You can never fall off the level**
`GameSession` checks whether the player fell off the bottom of the screen, but it never happens.
- Files: `src/glitch_runner/application/session.py`, `src/glitch_runner/domain/player.py`
- Hint: find the fall check in `session.py`, then look at `_clamp_to_screen` in `player.py`. Decide which behaviour the game should have: losing a life, or a solid floor.

### Level 4: Stretch

Bigger features that touch several files. Pair up if you like.

**4.1 Make F3 toggle debug mode**
The game has a debug view (hitboxes, FPS, player state) but it can only be switched on by editing `DEBUG_MODE` in `config.py`.
- Files: `config.py`, `presentation/input.py`, `presentation/renderer.py`, `presentation/sprites.py`
- Hint: `DEBUG_MODE` is a constant read when the program starts. It needs to become a setting that can change while the game runs.

**4.2 Pause screen**
Press P to pause and unpause.
- Files: `application/session.py`, `presentation/input.py`, `presentation/renderer.py`
- Hint: add a new value to `GameState`. Glitch timers use the clock, so think about what happens to them while paused.

**4.3 New enemy type**
For example, a "chaser" that walks towards the player.
- Files: `domain/enemy.py` (behaviour), `presentation/sprites.py` (how it looks), `domain/levels.py` (use it)
- Hint: start by adding a value to `EnemyType`.

**4.4 New glitch**
For example, "mirror mode" (left and right swapped) or "tiny player".
- Files: `domain/glitches.py` (what it does), `presentation/effects.py` (if it changes the screen)
- Hint: add a value to `GlitchKind`, then handle it in `_activate` and `_deactivate`. The HUD label is created automatically.

**4.5 Fix the standing jitter**
Turn on debug mode and stand still: "On Ground" flips between True and False.
- Files: `domain/player.py`
- Hint: pygame `Rect` positions are whole numbers, so small velocities get rounded away. Look at what happens when the player's bottom edge exactly touches a platform.

---

## Project Tour

### Where to change things

| I want to...                         | Look in                                      |
| ------------------------------------ | -------------------------------------------- |
| Tweak speed, gravity, jump height    | `src/glitch_runner/config.py`                |
| Add or edit a level                  | `src/glitch_runner/domain/levels.py`         |
| Change how the player moves          | `src/glitch_runner/domain/player.py`         |
| Change enemy behaviour               | `src/glitch_runner/domain/enemy.py`          |
| Add a new glitch                     | `src/glitch_runner/domain/glitches.py` and `presentation/effects.py` |
| Change scoring, lives, game flow     | `src/glitch_runner/application/session.py`   |
| Change what's drawn on screen        | `src/glitch_runner/presentation/renderer.py` |
| Change key bindings                  | `src/glitch_runner/presentation/input.py`    |
| Add sounds                           | `src/glitch_runner/infrastructure/audio.py` and `assets/sounds/` |

### How the code is organised

The code is split into layers so you can work on one part without understanding all of it:

```
src/glitch_runner/
├── domain/            # The game rules: how things move, collide and glitch
├── application/       # The game flow: menu, playing, game over, scoring, lives
├── infrastructure/    # Talking to the computer: loading images and playing sounds
├── presentation/      # What you see and press: drawing the screen, reading keys
├── assets/            # Images and sounds
├── config.py          # Numbers you can tweak
└── app.py             # Starts the game and connects everything
tests/                 # Tests that run without opening a window
```

One rule keeps this tidy: **`domain/` never draws or plays sound.** It only works out where things are and what happens. That's why its tests run without opening a window. If your change needs something on screen, the drawing part goes in `presentation/`.

### Each frame, in order

1. `app.py` reads key presses and passes them to the `GameSession`.
2. `GameSession.update()` moves the player and enemies, applies glitches, and checks for the exit, enemy hits and lives.
3. `GameRenderer.render()` draws everything to the screen.

This runs 60 times a second.

---

## Troubleshooting

**`uv: command not found`**
Close and reopen your terminal after installing uv.

**In Codespaces: the game doesn't show up**
The game draws to the Game Desktop, not to the editor. Open the **Ports** tab, click the globe next to **Game Desktop (6080)**, then click **Connect**. If port 6080 isn't listed, add it with **Forward a Port**.

**In Codespaces: keys don't do anything**
Click inside the game window on the Game Desktop first so it has focus.

**On your own computer: "No available video device"**
pygame needs a real screen. Run it on your own desktop, not over SSH. Use Codespaces if you can't. Tests work anywhere.

**No sound**
Check your volume and press M in case music is muted. Missing sound files are skipped silently.

**pygame fails to install**
Always use `uv run ...` and not plain `python`. uv uses the Python version in `.python-version`, which pygame supports.

**Tests fail before you've changed anything**
Run `uv sync` again, then `uv run pytest`. If it still fails, tell an organiser.

---

## For Organisers

### Codespaces setup

The `.devcontainer/` folder sets up each Codespace automatically:

- Python 3.12 and uv, with dependencies installed by `uv sync`
- A browser desktop (noVNC) on port 6080 so pygame has a screen to draw on
- The Python and Ruff VS Code extensions, with format on save
- Sound switched off (`SDL_AUDIODRIVER=dummy`), since Codespaces has no audio device

To make Codespaces start faster for everyone, turn on **prebuilds**: repository **Settings → Codespaces → Set up prebuild**, branch `master`.

Codespaces usage is billed to whoever creates the codespace (or to your organisation, if the repo belongs to one). Each participant needs a 2-core machine. Ask people to stop their codespace when they're done for the day.

### Building an executable

For a standalone version of the game:

```bash
uv run --group build pyinstaller GlitchRunner.spec
```

The executable is written to `dist/`.
