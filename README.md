# Forsaken-Style Roblox Game

A 4v1 horror game built with Roblox Studio and Luau.

**Parent-Son Project** | Started: 2026-01-28 | Target: 12-16 weeks

---

## Quick Start

### 1. Install Development Tools

| Tool | Status | Action Needed |
|------|--------|---------------|
| VS Code | Installed | Install Argon extension |
| Roblox Studio | Unknown | Install from [roblox.com](https://create.roblox.com) |
| Argon (VS Code) | Not Installed | `code --install-extension dervex.argon` |
| Argon (Plugin) | Unknown | Install in Roblox Studio |
| Blender | Not in PATH | Optional: Install from [blender.org](https://blender.org) |
| Git | Installed (v2.52.0) | Ready |

### 2. Install Argon Extension

```bash
code --install-extension dervex.argon
```

### 3. Install Argon Plugin in Roblox Studio

1. Open Roblox Studio
2. Go to [Creator Hub](https://create.roblox.com/marketplace/plugins/development)
3. Search "Argon" by dervex
4. Add to your account
5. Enable in Plugin Manager

### 4. Start Syncing

```bash
# In this folder
~/.local/bin/argon.exe serve
```

Then in Roblox Studio, click the Argon plugin → "Connect"

### 5. Verify Setup

Create a test Part in Studio's Workspace → should appear in `game/Workspace/`

---

## Project Structure

```
forsaken-style/
├── src/          # Source code (edit in VS Code)
│   ├── Server/   # Server-side scripts
│   ├── Client/   # Client-side scripts
│   └── Shared/   # Shared modules
├── game/         # Syncs to/from Roblox Studio
├── assets/       # Models, textures, audio
├── tests/        # TestEZ unit tests
└── docs/         # Project documentation
```

---

## Documentation

- [Product Specification](docs/2026-01-28-prodspec-roblox-forsaken-style-game.md)
- [Architecture](docs/architecture.md) - Tech stack, tools, networking
- [Testing Methodology](docs/testing.md)

---

## Development Workflow

1. **Son**: Works in Roblox Studio (visuals, map, gameplay feel)
2. **Parent**: Works in VS Code (game logic, systems)
3. **Argon**: Keeps both in sync automatically

---

## Phase 1: Foundation (Weeks 1-4)

- [ ] Parent installs and configures Argon
- [ ] Son learns Roblox Studio basics
- [ ] Both play Forsaken together, document mechanics
- [ ] Create basic gray-box map layout
- [ ] Implement basic game state machine (waiting)

---

## Commands

```bash
# Start sync server
argon serve

# Run tests (in Roblox Studio)
# View → Test Runner → Run All Tests

# Team Test (multiplayer simulation)
# Test → Team Test → Set 5 players → Start
```

---

**Waiting for son to get home before writing game code.** 🎮
