# Product Specification: Forsaken-Style Roblox Game

**Version:** 0.1 (Draft - Pending User Interview)
**Created:** 2026-01-28
**Status:** INCOMPLETE - Critical gaps identified

---

## Executive Summary

A simplified 4v1 asymmetrical horror game for Roblox, inspired by "Forsaken" mechanics. Developed as a parent-child collaboration project (parent: backend/logic, son age 10: visual design).

**Scope:** Single level MVP

---

## Critical Evaluation of GLM's Plan

### What's Good
- Clear collaboration model (parent/son roles defined)
- Phased MVP approach (one level first)
- Comprehensive research links
- Realistic 6-8 week timeline
- Covers automated + manual testing

### Critical Gaps Identified

| Gap Category | Issue | Severity |
|--------------|-------|----------|
| **Game Design** | No formal GDD - mechanics are vague | HIGH |
| **Player Experience** | Missing progression/unlock system | MEDIUM |
| **Audio** | No audio/music requirements | HIGH |
| **Platform** | Target platforms undefined | HIGH |
| **Performance** | No FPS/polygon budgets | MEDIUM |
| **Networking** | Server authority model undefined | HIGH |
| **Data** | No persistence strategy (DataStore) | MEDIUM |
| **Security** | No anti-cheat considerations | LOW (MVP) |
| **Monetization** | Strategy undefined | LOW (MVP) |
| **Accessibility** | No considerations listed | MEDIUM |
| **3D Tools** | Multiple tools listed, no decision | HIGH |
| **Son's Learning** | Roblox Studio curve underestimated | MEDIUM |

---

## 1. Game Overview

### 1.1 High Concept
[NEEDS CONFIRMATION]
- **Genre:** Asymmetrical horror (4v1)
- **Inspiration:** Forsaken, Dead by Daylight
- **Unique Selling Point:** TBD

### 1.2 Target Audience
- **Age range:** 10+ (similar to Forsaken)
- **Roblox experience level:** Any
- **Horror tolerance:** Match Forsaken intensity (jump scares, creepy atmosphere, chase tension)

### 1.3 Platform Targets
**MVP:** PC (Roblox desktop client) only
**Future:** Mobile (iOS/Android) support planned

- [x] PC (Roblox desktop client) - MVP
- [ ] Mobile (iOS/Android) - Post-MVP goal
- [ ] Console (Xbox) - Not planned
- [ ] VR - Not planned

---

## 2. Core Gameplay

### 2.1 Game Mode
**4v1 Asymmetrical Horror**
- 4 Survivors vs 1 Killer
- Survivors: Complete objectives, escape
- Killer: Hunt and eliminate survivors

### 2.2 Win Conditions

**Survivors Win:**
- ANY survivor escapes through exit gate, OR
- Time runs out (surviving players win)
- Note: Only alive/escaped survivors count as winners

**Killer Wins:**
- ALL survivors eliminated before time expires
- If even 1 survivor is alive when timer ends, killer loses

### 2.3 Round Structure
- **Round duration:** Configurable in settings menu (default: 8 minutes recommended)
- **Lobby wait time:** TBD (suggest 30-60 seconds)
- **Minimum players to start:** 5 (1 killer + 4 survivors)
- **Can spectate after elimination?:** TBD

### 2.4 Killer: "The Flash" (Speed Hunter)
From GLM's plan:
- Sprint Burst: 3x speed, 3 seconds, 15s cooldown
- Footprint Trail: 5 seconds visibility
- Passive: +10% movement speed

**Attack System:** Health bar system
- Survivors have visible health pool
- Multiple hits required to down
- Attack cooldown: TBD during balancing

**Note:** Health bar is more complex to implement but less intense than one-hit-down

### 2.5 Survivor Mechanics
- **Objectives:** Repair 3 generators to power exit gate
- **Hiding spots:** Can hide in designated locations
- **Sprint/stamina:** TBD during balancing
- **Healing:** Both self-heal AND teammate heal (encourages teamwork)

**Balance Values (TBD during development):**
- Health pool size: TBD
- Healing time: TBD (self vs team)
- Generator repair time: TBD
- Sprint stamina depletion rate: TBD

---

## 3. Level Design

### 3.1 Map Requirements
- **Theme:** TBD - Son will decide (creative ownership)
- **Size:** Small-medium (appropriate for 5 players, 3 generators)
- **Lighting:** Dark horror atmosphere (match Forsaken)
- **Floors:** Single floor (MVP simplicity)

### 3.2 Key Locations
- Spawn points (killer + survivors - separate areas)
- **3 generators** (confirmed for MVP)
- Exit gate (1 for MVP)
- Hiding spots (TBD based on map size)

[NEEDS CONFIRMATION]
- How many hiding spots?
- Line-of-sight blocking elements?
- Vertical gameplay (multiple floors)?

---

## 4. Audio Requirements

**Advantage:** Parent is an audio producer - custom audio capability in-house

### 4.1 Sound Effects
- [ ] Footsteps (different surfaces)
- [ ] Generator repair sounds
- [ ] Killer ability sounds
- [ ] Attack/hit sounds
- [ ] Heartbeat proximity indicator
- [ ] Environmental ambience

### 4.2 Music
- [ ] Menu music
- [ ] In-game tension music
- [ ] Chase music
- [ ] Victory/defeat stingers

### 4.3 Audio Strategy (Phased)
**Phase 1 (MVP):** Roblox audio library - placeholder sounds
**Phase 2 (Polish):** Custom audio production by parent

This allows fast prototyping while reserving custom audio for polish phase.

---

## 5. Visual Requirements

### 5.1 Art Style
[NEEDS INPUT]
- **Style:** ? (realistic? stylized? low-poly? blocky Roblox?)
- **Color palette:** ? (dark/muted? high contrast?)

### 5.2 Character Design
- **Character approach:** All custom models (killer AND survivors)
- **Killer visual design:** TBD (son's design)
- **Survivor models:** Custom designs, not Roblox avatars

**3D Creation Tool:** Blender + Blender MCP (FREE)
- Claude Code controls Blender via MCP
- Son describes concepts, Claude generates models
- Full Blender editing available for refinements

**Phased approach:**
1. Phase 1: Prototype with Roblox primitives/placeholders
2. Phase 2: Son describes concepts to Claude
3. Phase 3: Generate models via Blender MCP, export FBX to Roblox

### 5.3 Performance Budgets
[NEEDS INPUT]
- Target FPS: ? (30? 60?)
- Max triangle count per model: ?
- Texture resolution limits: ?
- Max parts in scene: ?

---

## 6. Technical Requirements

### 6.1 Networking Model
[MISSING FROM GLM'S PLAN]
- Server authority level?
- Client prediction?
- Lag compensation?

### 6.2 Data Persistence
[MISSING FROM GLM'S PLAN]
- Save player stats?
- Leaderboards?
- Progression unlocks?

### 6.3 Security
[MISSING FROM GLM'S PLAN]
- Exploit prevention priority?
- Server validation level?

---

## 7. UI/UX Requirements

### 7.1 HUD Elements
[NEEDS INPUT]
- Health display
- Stamina bar
- Objective progress
- Timer
- Minimap?
- Killer proximity indicator?

### 7.2 Menus
- Main menu
- Lobby/matchmaking
- Settings
- End-game scoreboard

---

## 8. Progression & Monetization

### 8.1 Progression System
[NEEDS INPUT]
- XP/leveling?
- Unlockable killers?
- Cosmetics?
- Achievements?

### 8.2 Monetization
[NEEDS INPUT]
- Free to play?
- Robux purchases?
- Game passes?
- Developer products?

---

## 9. Success Metrics

### 9.1 MVP Definition (Phase 1)
- **Goal:** Playable with friends in private server
- **Minimum features:** Basic mechanics working, 1 killer, 1 map
- **Quality bar:** Functional, not polished

### 9.2 Public Release (Phase 2)
- **Goal:** Polished enough for public Roblox listing
- **Quality bar:** Bug-free, complete UI, audio, balanced gameplay

---

## 10. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| Son loses interest | Medium | High | Keep tasks fun, visual-focused |
| Scope creep | High | High | Strict single-level MVP |
| Tool complexity | Medium | Medium | Simplest tools first |
| Networking bugs | High | High | Test early with Team Test |
| Learning curve | HIGH | Medium | Son is complete beginner |
| Extended timeline | HIGH | Medium | Only 2-4 hrs/week available |

---

## Appendix A: Interview Questions for User

### Game Design
1. What are the exact win/lose conditions?
2. Round duration and player counts?
3. Horror intensity level (for age appropriateness)?

### Technical
4. Target platforms (PC only or mobile too)?
5. Performance targets (FPS, device support)?
6. Data persistence needs (stats, progression)?

### Audio/Visual
7. Art style direction?
8. Audio source strategy?
9. Custom models or Roblox avatars?

### Scope
10. MVP definition - what's the bare minimum to call it "done"?
11. Post-MVP plans?

### Collaboration
12. Son's current Roblox Studio experience level? **ANSWERED: Complete beginner**
13. Weekly time commitment (parent + son)? **PENDING**

### Team Strengths Identified
- Parent: Audio production (professional), backend/logic, power user
- Son: Visual design interest, complete Roblox Studio beginner

---

*Document Status: DRAFT - Requires user interview to complete*
