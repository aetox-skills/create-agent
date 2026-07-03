# Agent Spec: [Name]

## v[0.1.0] — [YYYY-MM-DD]
Layer: [Quick / Standard / Deep / Critical]
Archetype: [System / Creative / Developer / Scribe / Specialist / Orchestrator / Researcher / Game]
Status: [Proposed / Approved / In Progress / Built / Approved via Quick]

> **Quick layer:** use [`templates/quick-spec.md`](quick-spec.md) instead.
>   This full template is for Standard+ layers only.
> **Diff spec (modifying existing):** include only changed fields.
>   Mark unchanged as `—` (no change).

---

## Identity

- **Name:**
- **Archetype:**
- **Role:** This agent [does X] for [user Y] so that [outcome Z].
- **Persona:**
  - **Tone:** [formal / casual / technical / playful / authoritative / warm]
  - **Example phrase:** "[what it actually sounds like in conversation]"
- **Knows context:**
- **Narrowest job:**

---

## Capabilities

| # | Capability | Trigger | Success Output | Failure Mode |
|---|-----------|---------|---------------|-------------|
| 1 | | | | |
| 2 | | | | |

### Critical Rules (Boundaries)

| Rule | Scenario | Consequence |
|------|----------|-------------|
| | | |

### Authority Level

Level: [L1 Read-only / L2 Suggest / L3 Modify in scope / L4 Execute validation / L5 Autonomous bounded]

**Allowed:**
-

**Requires approval:**
-

**Forbidden:**
-

### Stop Conditions

The agent must stop when:

- [ ] Requirement is unclear
- [ ] Requested action exceeds scope
- [ ] Another agent owns the responsibility
- [ ] Required input is missing
- [ ] Safety, data, permission, or cost risk appears

### Delegation

| When | To Whom | What Passed |
|------|---------|-------------|
| | | |

---

## Tool Mapping

| Capability | Tool / Category | Existing? | Gap | Recommendation |
|-----------|----------------|-----------|-----|----------------|
| | | ✅ / ❌ | | |

---

## Input / Output

- **Input format:**
- **Output format:**
- **Timing:** [sync / async]
- **Error output:**

### Deliverables

| Deliverable | Format | Template/Idea |
|-------------|--------|---------------|
| | | |

---

## Ecosystem

- **Platform:**
- **Placement:** [subagent / plugin / standalone / MCP]
- **Team:** [alone / supervised / peer]
- **Communicates with:**

| Agent/Service | Interaction | Format |
|---------------|------------|--------|
| | | |

- **Dependencies:**

| Dep | Required? | Fallback? |
|-----|-----------|-----------|
| | | |

### Overlap Detection

| Area | Existing Owner | Conflict? | Resolution |
|------|----------------|-----------|------------|
| | | | |

---

## Memory & Learning

- **Session memory:**
- **Persistent memory:**
- **Knowledge accumulation:**
- **Forgetting:**

---

## Lifecycle

- **Trigger:**
- **Auto-load:** [yes / no]
- **Duration:**
- **Maintenance:**
- **Retirement plan:**

---

## Risk & Safety (Layer 3+)

| Failure | Probability | Impact | Mitigation |
|---------|------------|--------|------------|
| | | | |

### Sensitive Data
-

### Safety Gates
- [ ] Read-only by default?
- [ ] Human approval required?
- [ ] Audit log?
- [ ] Rollback plan?

---

## Success Metrics

Optional. Add as many as useful — 1 is fine.

| Metric | Type | Target | Measure | Alert If |
|--------|------|--------|---------|----------|
| | | | | |

### Cost Model (Layer 4+)
- **Per-call budget:**
- **Per-session estimate:**
- **Monthly projection:**
- **Cache hit target:**
- **Alert threshold:**

---

## Agent Test Plan

| Capability | Test Prompt | Expected Output | Edge Cases |
|-----------|-------------|-----------------|------------|
| | | | |

---

## Approval

| Item | Approved? | Approver | Date |
|------|-----------|----------|------|
| Spec | | | |
| Cost | | | |
| Safety | | | |

---

## Iteration Log

| Iteration | Change | Before | After | Test Result | Known Issues |
|-----------|--------|-------|-------|-------------|--------------|
| 1 | | | | | |
| 2 | | | | | |

---

## Notes

-
