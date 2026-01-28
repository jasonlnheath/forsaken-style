# Testing Methodology: Forsaken-Style Roblox Game

**Version:** 1.0
**Created:** 2026-01-28

---

## 1. Testing Strategy Overview

### 1.1 Testing Pyramid

```
                    ┌─────────────┐
                    │   Manual    │  ← Son + Parent playtesting
                    │ Playtesting │     (Fun factor, balance)
                   ─┴─────────────┴─
                  ┌─────────────────┐
                  │   Integration   │  ← Team Test in Studio
                  │     Tests       │     (Multiplayer flows)
                 ─┴─────────────────┴─
                ┌───────────────────────┐
                │      Unit Tests       │  ← TestEZ
                │   (Logic validation)  │     (Automated, fast)
               ─┴───────────────────────┴─
```

### 1.2 Testing Tools Summary

| Tool | Type | Automation | Use Case |
|------|------|------------|----------|
| **TestEZ** | Unit | Automated | Logic validation |
| **Team Test** | Integration | Manual | Multiplayer simulation |
| **roblox-screenshot** | Visual | Semi-automated | Visual regression |
| **Manual Playtest** | E2E | Manual | Fun factor, balance |

---

## 2. Unit Testing (TestEZ)

### 2.1 What to Test

| Module | Test Focus |
|--------|------------|
| `KillerLogic` | Damage calculation, ability cooldowns, speed modifiers |
| `SurvivorLogic` | Health/stamina, repair progress, escape conditions |
| `GameLoop` | Round start/end, win conditions, timer logic |
| `Config` | Balance values within expected ranges |

### 2.2 Example Tests

```lua
-- tests/KillerLogic.test.lua
return function()
    describe("KillerLogic", function()
        describe("Sprint Burst ability", function()
            it("should have 3 second duration", function()
                local duration = KillerLogic.getSprintDuration()
                expect(duration).to.equal(3)
            end)

            it("should have 15 second cooldown", function()
                local cooldown = KillerLogic.getSprintCooldown()
                expect(cooldown).to.equal(15)
            end)

            it("should not allow sprint during cooldown", function()
                KillerLogic.activateSprint()
                local canSprint = KillerLogic.canSprint()
                expect(canSprint).to.equal(false)
            end)

            it("should allow sprint after cooldown expires", function()
                KillerLogic.activateSprint()
                -- Simulate cooldown passing
                KillerLogic._debugSetCooldown(0)
                local canSprint = KillerLogic.canSprint()
                expect(canSprint).to.equal(true)
            end)
        end)

        describe("Attack damage", function()
            it("should deal expected damage per hit", function()
                local damage = KillerLogic.getAttackDamage()
                expect(damage).to.be.a("number")
                expect(damage).to.be.greaterThan(0)
            end)
        end)
    end)
end
```

```lua
-- tests/GameLoop.test.lua
return function()
    describe("GameLoop", function()
        describe("Role assignment", function()
            it("should assign exactly 1 killer for 5 players", function()
                local roles = GameLoop.assignRoles(5)
                local killerCount = 0
                for _, role in pairs(roles) do
                    if role == "Killer" then
                        killerCount = killerCount + 1
                    end
                end
                expect(killerCount).to.equal(1)
            end)

            it("should assign 4 survivors for 5 players", function()
                local roles = GameLoop.assignRoles(5)
                local survivorCount = 0
                for _, role in pairs(roles) do
                    if role == "Survivor" then
                        survivorCount = survivorCount + 1
                    end
                end
                expect(survivorCount).to.equal(4)
            end)
        end)

        describe("Win conditions", function()
            it("should detect killer win when all survivors eliminated", function()
                local gameState = {
                    aliveSurvivors = 0,
                    escapedSurvivors = 0,
                    generatorsRepaired = 2,
                    generatorsRequired = 3,
                }
                local winner = GameLoop.checkWinCondition(gameState)
                expect(winner).to.equal("Killer")
            end)

            it("should detect survivor win when all escape", function()
                local gameState = {
                    aliveSurvivors = 0,
                    escapedSurvivors = 4,
                    generatorsRepaired = 3,
                    generatorsRequired = 3,
                }
                local winner = GameLoop.checkWinCondition(gameState)
                expect(winner).to.equal("Survivors")
            end)
        end)
    end)
end
```

### 2.3 Running Tests

```bash
# In Roblox Studio
# 1. Open Test Runner (View → Test Runner)
# 2. Click "Run All Tests"

# In CI/CD (GitHub Actions)
# Automated via workflow (see .github/workflows/test.yml)
```

### 2.4 Test Coverage Goals

| Phase | Coverage Target |
|-------|-----------------|
| MVP (Phase 1) | Core logic only (GameLoop, KillerLogic, SurvivorLogic) |
| Public Release (Phase 2) | All game logic + edge cases |

---

## 3. Integration Testing (Team Test)

### 3.1 What Team Test Validates

- Server-client replication
- Multiplayer synchronization
- Network latency handling
- Role assignment across clients

### 3.2 Team Test Procedure

1. Open game in Roblox Studio
2. Go to **Test** tab → **Team Test**
3. Set player count to 5
4. Click **Start**
5. Each window represents a different player
6. Manually verify gameplay flows

### 3.3 Integration Test Checklist

**Round Flow:**
- [ ] Lobby fills to 5 players
- [ ] Game starts after countdown
- [ ] 1 player assigned Killer, 4 assigned Survivor
- [ ] Timer counts down correctly
- [ ] Win condition triggers properly

**Networking:**
- [ ] Player positions sync across clients
- [ ] Health changes replicate to all
- [ ] Generator progress visible to all
- [ ] Killer abilities work for all clients

**Edge Cases:**
- [ ] Player disconnect mid-game
- [ ] Player join during active round
- [ ] Rapid input spam handling

---

## 4. Visual Testing (Semi-Automated)

### 4.1 Screenshot Capture

**Tool:** roblox-screenshot (Corecii)

**Purpose:** Visual regression detection between builds

**Workflow:**
```
1. Capture baseline screenshots
2. Make code changes
3. Capture new screenshots
4. Compare for visual regressions
5. Human review of differences
```

### 4.2 Screenshot Scenarios

| Scenario | Camera Position | What to Verify |
|----------|-----------------|----------------|
| `killer_spawn` | Killer POV at spawn | Model renders, UI shows |
| `survivor_spawn` | Survivor POV at spawn | Model renders, team visible |
| `generator_progress` | At generator | Progress bar, interaction prompt |
| `exit_gate_closed` | At exit gate | Gate closed visual |
| `exit_gate_open` | At exit gate | Gate open visual |
| `killer_ability` | During sprint burst | Speed trails, effects |

### 4.3 Visual Test Decision

**For MVP:** Skip automated visual testing
**For Public Release:** Implement screenshot comparison

**Rationale:** Manual visual inspection is sufficient for single-person team with limited time.

---

## 5. Manual Playtesting

### 5.1 Playtest Roles

| Tester | Focus | Frequency |
|--------|-------|-----------|
| **Son** | Fun factor, scary enough?, visuals | Every session |
| **Parent** | Technical bugs, exploits, performance | Every session |
| **Friends** | Fresh perspective, balance | Weekly (after Week 8) |

### 5.2 Playtest Checklist by Phase

**Week 4 Checkpoint:**
- [ ] Can walk around map
- [ ] Can interact with generators (E key)
- [ ] Timer visible
- [ ] Killer can attack survivors
- [ ] Basic win/lose screen appears

**Week 8 Checkpoint:**
- [ ] Complete round plays start to finish
- [ ] Killer abilities feel balanced
- [ ] Survivors can hide effectively
- [ ] Generator repair feels satisfying
- [ ] Is it scary enough? (Match Forsaken)

**Week 12 Checkpoint:**
- [ ] Multiplayer stable with 5 players
- [ ] No game-breaking bugs
- [ ] UI polished and readable
- [ ] Audio enhances atmosphere
- [ ] Would son want to play again?

**Week 16 Checkpoint (Release):**
- [ ] Stress tested with friends
- [ ] Balance feels fair for both sides
- [ ] No exploits found
- [ ] Ready for public listing

### 5.3 Playtest Session Template

```markdown
## Playtest Report
**Date:** YYYY-MM-DD
**Testers:** Parent, Son, [Friends]
**Build Version:** v0.X.X

### What Worked
- [List positive observations]

### Issues Found
| Issue | Severity | Reproducible? | Notes |
|-------|----------|---------------|-------|
| [desc] | High/Med/Low | Yes/No | [details] |

### Balance Observations
- Killer: Too strong / Just right / Too weak
- Survivors: Too strong / Just right / Too weak
- Round length: Too short / Just right / Too long

### Fun Factor (Son's Rating)
- Scale 1-10: ___
- What was most fun?
- What was least fun?

### Action Items
- [ ] [Fix description]
- [ ] [Balance adjustment]
```

---

## 6. Performance Testing

### 6.1 Performance Metrics to Monitor

| Metric | Target | How to Measure |
|--------|--------|----------------|
| **FPS** | 60+ | F9 debug menu |
| **Memory** | <500 MB | F9 debug menu |
| **Network** | <10 KB/s | F9 debug menu |
| **Load time** | <10s | Stopwatch |

### 6.2 Performance Test Procedure

1. Open game in Team Test (5 players)
2. Press F9 to open debug menu
3. Navigate to **Performance** tab
4. Play full round while monitoring metrics
5. Document any drops below targets

---

## 7. Regression Testing

### 7.1 Regression Test Suite

Before each milestone, run the following:

**Automated (TestEZ):**
- [ ] All unit tests pass

**Manual Smoke Test:**
- [ ] Game launches without errors
- [ ] Can start a round
- [ ] Killer can attack
- [ ] Survivors can repair
- [ ] Exit gate opens after generators
- [ ] Win condition triggers

### 7.2 Bug Tracking

**Simple tracking (MVP):**
- GitHub Issues in project repository
- Labels: `bug`, `balance`, `enhancement`, `wontfix`

---

## 8. CI/CD Test Integration

### 8.1 GitHub Actions Workflow

```yaml
# .github/workflows/test.yml
name: Run Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Install dependencies
        run: |
          # Install Rojo CLI
          cargo install rojo

      - name: Build project
        run: rojo build -o game.rbxl

      - name: Run linter
        run: selene src/

      # Note: Full TestEZ requires Roblox Studio
      # Consider run-in-roblox for automated execution
```

### 8.2 Pre-Commit Checks

- [ ] Lua linting (Selene)
- [ ] Type checking (Luau LSP)
- [ ] Basic syntax validation

---

## 9. Testing Schedule

| Week | Testing Focus |
|------|---------------|
| 1-4 | Unit tests for core logic as built |
| 5-8 | Integration tests (Team Test), first playtests |
| 9-12 | Balance playtests, performance testing |
| 13-16 | Full regression, friend playtests, release prep |

---

## Appendix A: TestEZ Quick Reference

```lua
-- Describe a test suite
describe("ModuleName", function()
    -- Describe a feature
    describe("featureName", function()
        -- Individual test
        it("should do something", function()
            expect(value).to.equal(expected)
        end)
    end)
end)

-- Common matchers
expect(value).to.equal(expected)
expect(value).to.be.a("string")
expect(value).to.be.ok()          -- truthy
expect(value).never.to.be.ok()    -- falsy
expect(value).to.be.greaterThan(n)
expect(value).to.be.lessThan(n)
expect(func).to.throw()
```

---

## Appendix B: Known Limitations

| Limitation | Impact | Workaround |
|------------|--------|------------|
| Cannot automate "fun factor" testing | Requires human judgment | Regular playtests with son |
| TestEZ requires Studio | Cannot run in pure CI | Use linting + manual test |
| No automated multiplayer testing | Integration tests are manual | Team Test feature |
| Visual testing is manual | Time-consuming | Limit to major milestones |

---

*Document maintained by: Parent (tech lead)*
*Last reviewed: 2026-01-28*
