# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains two standalone HTML5 browser games:

1. **Sheep Jump Game** (`sheepjump.html`) - An endless runner where players control a sheep jumping over obstacles
2. **Tic Tac Toe** (`tictactoe.html`) - A classic two-player tic-tac-toe game

Both games are self-contained single-file applications with embedded CSS and JavaScript. No build process, dependencies, or external libraries are required.

## Git Workflow

**IMPORTANT**: As you complete work, commit changes to Git and push to GitHub regularly. This ensures no work is lost and maintains a clean history of progress.

### Commit Guidelines

- Commit after completing each discrete unit of work (bug fix, feature addition, refactoring)
- Write clear, descriptive commit messages that explain what changed and why
- Use conventional commit format: `<type>: <description>` (e.g., "feat: add pause button to sheep jump game", "fix: correct tic-tac-toe win detection")
- Push commits to GitHub immediately after committing to ensure work is backed up

### Example Workflow

```bash
# After making changes
git add <modified-files>
git commit -m "feat: add high score tracking to sheep jump game"
git push origin master

# For multiple related files
git add .
git commit -m "refactor: extract collision detection into separate function"
git push origin master
```

Always push your commits to avoid losing work between sessions.

## Running the Games

Open the HTML files directly in a web browser:

```bash
# macOS
open sheepjump.html
open tictactoe.html

# Linux
xdg-open sheepjump.html
xdg-open tictactoe.html

# Or use a simple HTTP server if needed
python3 -m http.server 8000
# Then navigate to http://localhost:8000/sheepjump.html or tictactoe.html
```

## Architecture

### Sheep Jump Game (`sheepjump.html`)

**Core Game Loop**: Uses `requestAnimationFrame` for smooth 60fps rendering with delta-time physics calculations.

**Physics System**:
- Gravity-based jumping mechanics with velocity and acceleration
- Constants defined at top of script (lines 170-197): `GRAVITY`, `JUMP_VELOCITY`, obstacle speeds
- Physics calculations in `updateSheep()` (lines 224-240)

**Difficulty Progression**:
- Speed increases every 5 points (`SPEED_INCREASE_INTERVAL`)
- Obstacle spawn rate increases every 10 points (`SPAWN_DECREASE_INTERVAL`)
- Both capped at maximum values to maintain playability

**Collision Detection**: Uses AABB (Axis-Aligned Bounding Box) collision with padding for fair hit detection (lines 315-336)

**Game State Management**: Single `gameActive` boolean controls the entire game loop. State reset handled in `restart()` function (lines 376-398)

### Tic Tac Toe Game (`tictactoe.html`)

**Game State**: Managed through three core variables:
- `gameBoard` array (9 elements) tracks cell states
- `currentPlayer` toggles between 'X' and 'O'
- `gameActive` boolean prevents moves after game ends

**Win Detection**: Pre-defined `winningConditions` array contains all 8 possible winning combinations (3 rows, 3 columns, 2 diagonals). Checked after each move in `checkResult()` (lines 161-196).

**UI Updates**: CSS classes (`x`, `o`, `winner`) applied dynamically for styling. Winning cells highlighted with pulse animation.

## Code Patterns

### Single-File Architecture
All games follow this structure:
1. HTML structure
2. Embedded `<style>` block with CSS
3. Embedded `<script>` block with vanilla JavaScript
4. No external dependencies or imports

When modifying games, maintain this self-contained pattern.

### DOM Element References
Elements are cached at initialization (e.g., `const sheep = document.getElementById('sheep')`). Update references if adding new interactive elements.

### Event Handling
- Sheep Jump: Keyboard events for spacebar (`keydown` listener)
- Tic Tac Toe: Click events on cells and reset button

### Position/Style Updates
Games manipulate DOM styles directly (e.g., `sheep.style.bottom = sheepY + 'px'`). No canvas element is used despite the class name `.game-canvas`.

## Testing

No automated test suite exists. Test manually by:

1. Opening games in browser
2. Verifying game mechanics (jumping, clicking, win conditions)
3. Testing edge cases (rapid clicking, holding spacebar, draw conditions)
4. Checking responsive behavior and animations
5. Testing in multiple browsers (Chrome, Firefox, Safari)
