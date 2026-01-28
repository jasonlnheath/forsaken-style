# NEXT STEPS - Run /continue to Begin Development

**Last Updated:** 2026-01-28
**Status:** Project initialized, awaiting son's return to begin coding
**Repository:** https://github.com/jasonlnheath/forsaken-style

---

## Quick Start Commands

```bash
# Navigate to project
cd c:/dev/robloxdev/game/forsaken-style

# Start Argon sync server
~/.local/bin/argon.exe serve

# Open VS Code in this folder
code .
```

---

## Before Each Development Session

### 1. Start Argon Server
```bash
cd c:/dev/robloxdev/game/forsaken-style
~/.local/bin/argon.exe serve
```
- Keep this terminal window open
- Server runs on http://localhost:8000

### 2. Open Roblox Studio
- Open your place file
- Click **Argon** plugin → **Connect**
- Verify it shows "Connected" (green status)

### 3. Verify Sync
- Create a test script in `src/Server/` in VS Code
- Should appear in Studio immediately
- Edit script in Studio
- Should update in VS Code within 3 seconds

---

## Phase 1: Foundation (Weeks 1-4)

When your son is ready, start here:

### Week 1 Goals
- [ ] Son: Explore Roblox Studio interface
- [ ] Son: Learn to create Parts, move camera, use tools
- [ ] Parent: Review `docs/architecture.md` and `docs/testing.md`
- [ ] Together: Play Forsaken, document what makes it fun/scary

### Week 2 Goals
- [ ] Son: Build a gray-box map layout (no details, just shapes)
- [ ] Parent: Create basic game state machine (waiting to code)
- [ ] Together: Decide on map theme (asylum? forest? abandoned facility?)

### Week 3-4 Goals
- [ ] Parent: Implement lobby/waiting state
- [ ] Son: Add basic lighting and atmosphere to map
- [ ] Together: Test basic player movement

---

## Development Workflow

### Parent (You) - VS Code Focus
```
src/
├── Server/          # Game logic, state machine
│   ├── GameLoop.server.lua      (waiting to create)
│   ├── KillerLogic.server.lua   (waiting to create)
│   └── SurvivorLogic.server.lua (waiting to create)
├── Client/          # UI, input, visual effects
│   ├── UIController.client.lua  (waiting to create)
│   └── Audio.client.lua         (waiting to create)
└── Shared/          # Shared modules
    ├── Config.lua               (waiting to create)
    ├── Constants.lua            (waiting to create)
    └── Types.lua                (waiting to create)
```

### Son (Your Son) - Roblox Studio Focus
```
Workspace/Map/      # Build the horror map here
  - Generators      (repair objectives)
  - Exit gates      (escape routes)
  - Hiding spots    (lockers, under beds)
  - Atmosphere      (lighting, fog, sounds)
```

---

## Documentation Reference

| Document | Purpose | Location |
|----------|---------|----------|
| Product Spec | Game design decisions | `docs/2026-01-28-prodspec-roblox-forsaken-style-game.md` |
| Architecture | Tech stack, tools, networking | `docs/architecture.md` |
| Testing | How to test the game | `docs/testing.md` |
| Plan | Original project plan | `~/.claude/plans/velvet-petting-whisper.md` |

---

## Argon Sync Settings (Do Not Change)

```
✅ Two-way sync: ENABLED
✅ Only code mode: ENABLED
❌ Syncback properties: DISABLED
```

This ensures:
- Scripts sync both directions
- Visual objects stay in Studio (not cluttering files)
- Clean separation of concerns

---

## Git Workflow

```bash
# Pull latest changes
git pull

# Work on features...

# Stage and commit
git add -A
git commit -m "Describe your changes"

# Push to GitHub
git push
```

---

## When Ready to Code: Run /continue

When your son is home and ready to start development, simply type:

```
/continue
```

This will:
1. Recall all project context
2. Show you what's next
3. Help you start coding Phase 1

---

## Current Project Status

| Component | Status |
|-----------|--------|
| Git repository | ✅ Initialized & pushed |
| Folder structure | ✅ Created |
| Argon CLI | ✅ Installed (v2.0.27) |
| Argon sync | ✅ Configured & tested |
| Documentation | ✅ Complete |
| Game code | ⏳ Waiting for son |
| Map/visuals | ⏳ Waiting for son |

---

## Contact / Help

- GitHub: https://github.com/jasonlnheath/forsaken-style
- Plan file: `~/.claude/plans/velvet-petting-whisper.md`

**Run `/continue` when ready to begin coding!**
