# Architecture Document: Forsaken-Style Roblox Game

**Version:** 1.0
**Created:** 2026-01-28
**Last Updated:** 2026-01-28

---

## 1. Tech Stack Overview

| Layer | Primary Tool | Backup (Plan B) | Backup (Plan C) |
|-------|-------------|-----------------|-----------------|
| **IDE** | VS Code | Roblox Studio (visual) | - |
| **Sync** | Argon | Rojo | - |
| **Language** | Luau | - | - |
| **3D Modeling** | Blender + MCP | Tripo AI (free tier) | Roblox Primitives |
| **Version Control** | Git + GitHub | - | - |
| **Testing** | TestEZ | Manual playtesting | - |
| **CI/CD** | GitHub Actions | - | - |

---

## 2. Tool Decisions & Rationale

### 2.1 Sync Tool: Argon (Primary) vs Rojo (Backup)

**Decision: Argon**

| Criteria | Argon | Rojo |
|----------|-------|------|
| **Learning Curve** | Lower | Higher |
| **Two-Way Sync** | Native | Supported |
| **VS Code Integration** | Excellent | Good |
| **Documentation** | Good | Excellent |
| **Community Size** | Growing | Larger |
| **Beginner-Friendly** | Yes | Less so |

**Rationale:** With son being a complete beginner and only 2-4 hours/week available, minimizing friction is critical. Argon's simpler setup and beginner-friendly design outweighs Rojo's larger community.

**Plan B: Rojo**
- Use if Argon has issues or lacks specific features
- Industry standard, more tutorials available
- [Rojo Documentation](https://rojo.space/docs/v7/)

**Sources:**
- [Rojo Alternatives](https://rojo.space/docs/v7/rojo-alternatives/)
- [Argon DevForum Thread](https://devforum.roblox.com/t/argon-full-featured-tool-for-roblox-development/2021776)

---

### 2.2 3D Modeling: Blender + Blender MCP (Primary)

**Decision: Blender with MCP (Model Context Protocol) for AI-assisted modeling**

| Criteria | Blender MCP | Tripo AI | Meshy | Roblox Primitives |
|----------|-------------|----------|-------|-------------------|
| **Price** | FREE | $12/mo | $16/mo | Free |
| **Workflow** | Prompt-based | Web UI | Web UI | Manual |
| **Claude Integration** | Native | None | None | None |
| **Learning Curve** | Low (prompts) | Low | Low | Lowest |
| **Customization** | Full (Python) | Limited | Limited | Limited |
| **Local/Offline** | Yes | No | No | Yes |

**Rationale:**
- **FREE** - No monthly subscription costs
- Claude Code can directly control Blender via MCP
- Son describes what he wants, Claude generates it
- Full Blender power available for refinements
- Integrates with Hunyuan3D (free) or Hyper3D (limited free) for AI mesh generation
- Parent's existing Python knowledge transfers

**Workflow:**
```
1. Son describes character/object concept
2. Claude Code sends prompt to Blender via MCP
3. Blender creates/modifies 3D model
4. Review and iterate via more prompts
5. Export as FBX
6. Import to Roblox Studio (verify under 20k triangles)
```

**Components (all FREE):**
- Blender 3.0+ (free, open source)
- Blender MCP addon (free, MIT license)
- Hunyuan3D integration (free AI mesh generation)
- Poly Haven assets (free models, textures, HDRIs)

**Setup Requirements:**
- Blender 3.0 or newer
- Python 3.10+
- `uv` package manager
- Claude Desktop, Cursor, or VS Code with MCP config

**GitHub:** [ahujasid/blender-mcp](https://github.com/ahujasid/blender-mcp)

**Plan B: Tripo AI Free Tier**
- Pros: Web-based, no setup, limited free generations
- Cons: Limited free tier, no Claude integration
- Use for: Quick prototypes when Blender unavailable

**Plan C: Roblox Studio Primitives**
- Pros: Free, no external tools, native
- Cons: Limited to blocky designs
- Use for: Quick in-Studio prototyping

**Roblox Import Constraints:**
- **Max triangles:** 20,000 per mesh
- **Format:** FBX (recommended) or OBJ
- **Textures:** Applied separately after import
- **Scale:** 1 unit = 1 stud (humanoid ~5 studs tall)

**Sources:**
- [Blender MCP GitHub](https://github.com/ahujasid/blender-mcp)
- [Roblox Modeling Specifications](https://create.roblox.com/docs/art/modeling/specifications)
- [Roblox 3D Import Forum](https://devforum.roblox.com/t/what-are-the-limitations-to-modeling-and-importing-to-roblox/2453057)

---

### 2.3 Testing Framework: TestEZ

**Decision: TestEZ for unit testing**

**Rationale:**
- Community standard for Roblox Lua testing
- Fast, repeatable tests
- Integrates with CI/CD
- Validates game logic before playtesting

**Alternative: OpenGameEval**
- Better for AI/LLM evaluation
- Overkill for this project's scope
- Not a substitute for playtesting

---

### 2.4 CI/CD: GitHub Actions + place-ci-cd-demo Pattern

**Decision: GitHub Actions**

**Workflow:**
```
Git Push → GitHub Actions → Argon Build → Roblox Cloud Upload
```

**Alternative: Manual publishing**
- Simpler for MVP
- Use automated CI/CD for public release phase

---

## 3. Project Structure

```
forsaken-style/
├── .github/
│   └── workflows/
│       ├── test.yml           # TestEZ unit tests
│       └── publish.yml        # Automated publishing
├── src/
│   ├── Server/                # Server scripts (parent)
│   │   ├── GameLoop.server.lua
│   │   ├── KillerLogic.server.lua
│   │   ├── SurvivorLogic.server.lua
│   │   └── DataStore.server.lua
│   ├── Client/                # Client scripts
│   │   ├── UIController.client.lua
│   │   ├── InputHandler.client.lua
│   │   └── Audio.client.lua
│   └── Shared/                # Shared modules
│       ├── Config.lua
│       ├── Types.lua
│       └── Constants.lua
├── game/                      # Synced to/from Roblox Studio
│   ├── ServerScriptService/
│   ├── ReplicatedStorage/
│   ├── StarterPlayer/
│   ├── Workspace/             # Son's visual work area
│   │   └── Map/
│   └── StarterGui/
├── assets/
│   ├── models/                # FBX exports from Tripo AI
│   ├── textures/              # Texture files
│   └── audio/                 # Custom audio (Phase 2)
├── tests/
│   ├── GameLoop.test.lua
│   ├── KillerLogic.test.lua
│   └── SurvivorLogic.test.lua
├── docs/
│   ├── architecture.md        # This file
│   ├── prodspec.md            # Product specification
│   └── testing.md             # Testing methodology
├── default.project.json       # Argon project config
└── README.md
```

---

## 4. Networking Architecture

### 4.1 Server Authority Model

**Decision: Server-authoritative with client prediction**

**Server Responsibilities:**
- Game state management (round timer, win conditions)
- Player role assignment (killer vs survivor)
- Damage/health calculations
- Generator progress tracking
- Anti-exploit validation

**Client Responsibilities:**
- Input handling
- UI rendering
- Visual effects
- Sound playback
- Movement prediction (lag compensation)

### 4.2 Replication Strategy

| Data | Replication | Notes |
|------|-------------|-------|
| Player health | Server → All | Visible health bars |
| Generator progress | Server → All | Shared objective |
| Killer abilities | Server → Killer | Ability cooldowns |
| Player positions | Server → All | Required for gameplay |
| Game timer | Server → All | Synchronized countdown |

---

## 5. Data Architecture

### 5.1 DataStore Usage (Post-MVP)

**MVP:** No persistence (play sessions only)

**Post-MVP:**
- Player stats (wins, losses, playtime)
- Unlocks (if progression system added)
- Settings preferences

**DataStore Structure:**
```lua
{
  UserId = {
    stats = {
      gamesPlayed = 0,
      gamesWonAsSurvivor = 0,
      gamesWonAsKiller = 0,
    },
    settings = {
      musicVolume = 0.5,
      sfxVolume = 1.0,
    }
  }
}
```

---

## 6. Security Considerations

### 6.1 MVP Security (Minimal)
- Server-authoritative game state
- Basic sanity checks on client input

### 6.2 Public Release Security
- Validate all client actions server-side
- Rate limit ability usage
- Detect speed/teleport exploits
- Report suspicious behavior

---

## 7. Performance Targets

| Metric | Target | Notes |
|--------|--------|-------|
| **FPS** | 60 (PC) | Desktop-only for MVP |
| **Max triangles/model** | 10,000 | Conservative under 20k limit |
| **Max parts in scene** | 5,000 | Single small map |
| **Network tick rate** | 30 Hz | Standard for Roblox |

---

## 8. Timeline Adjustment

**GLM's Estimate:** 6-8 weeks (assumes 5-8 hrs/week)
**Revised Estimate:** 12-16 weeks (based on 2-4 hrs/week + beginner learning curve)

### Adjusted Milestones

| Phase | Original | Revised | Activities |
|-------|----------|---------|------------|
| Foundation | Week 1-2 | Week 1-4 | Setup, learning, basic map |
| Core Mechanics | Week 3-4 | Week 5-8 | Game logic, objectives |
| Killer Abilities | Week 5 | Week 9-10 | Combat, abilities |
| Survivor Systems | Week 6 | Week 11-12 | Health, stamina, hiding |
| Polish | Week 7-8 | Week 13-16 | Audio, visuals, testing |

---

## Appendix A: Tool Installation

### Argon Setup
```bash
# Install VS Code extension
code --install-extension dervex.argon

# Install Roblox plugin from Roblox Creator Hub
# Search: "Argon" by dervex
```

### Blender MCP Setup
1. Install Blender 3.0+ from [blender.org](https://blender.org)
2. Download `addon.py` from [blender-mcp](https://github.com/ahujasid/blender-mcp)
3. Install addon: Blender → Edit → Preferences → Add-ons → Install
4. Configure MCP in Claude Desktop/VS Code
5. Start Blender addon server
6. Use Claude prompts to create models
7. Export as FBX, import to Roblox Studio

### TestEZ Setup
```lua
-- ReplicatedStorage/Packages/TestEZ
-- Install via Wally or manual download from GitHub
```

---

## Appendix B: Decision Log

| Date | Decision | Rationale |
|------|----------|-----------|
| 2026-01-28 | Argon over Rojo | Beginner-friendly, limited time |
| 2026-01-28 | Blender MCP over Tripo AI | FREE, Claude integration, full control |
| 2026-01-28 | PC-only MVP | Simplify scope |
| 2026-01-28 | Single floor map | Reduce complexity |
| 2026-01-28 | Health bar system | More complex but less intense |

---

*Document maintained by: Parent (tech lead)*
*Last reviewed: 2026-01-28*
