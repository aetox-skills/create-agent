---
name: create-agent
description: >-
  Scope-first AI agent design discipline. Requires structured interview before
  creating or modifying any AI agent. Supports 4 depth layers (Sketch, Standard,
  Deep, Critical) with checkpoint gates, escalation rules, and an Agent Spec
  output. Every layer includes a minimum-viable scope validation.
  Use when asked to design, create, modify, or define a new AI agent.
  Do not skip scope validation — even for small changes.
---

# Create Agent

## Purpose

Design AI agents with a scope-first discipline. Before writing any prompt,
config, or tool binding, you must pass a structured scope interview. The depth
of the interview depends on the complexity of the agent being designed.

This is **not** a prompt-writing skill. It is a **scope-validation** skill.
The output is a validated **Agent Spec** — a blueprint that another AI or human
can use to build the agent correctly.

---

## Iron Rules

1. **Scope before build** — No code, no prompt, no tool config until scope is
   validated. Zero exceptions.
2. **Depth before breadth** — Choose the right layer first. Do not default to
   the deepest layer. Do not skip layers.
3. **Codebase-first** — If the answer to a question exists in config, docs,
   or existing agents — go find it. Do not ask.
4. **One question at a time** — Ask, wait for answer, then ask the next.
   Do not dump 10 questions at once.
5. **Recommend, don't open-ended** — Every question must include a
   recommended answer. Give something to react to.
6. **State explicit** — Never proceed on implicit understanding.
   If it's not written, it's not agreed.
7. **Stop on signal** — When the user says "enough" or "stop" — stop.
   Do not argue, do not push for more.
8. **Output an Agent Spec** — Every session produces a validated spec, even
   at Sketch layer.

---

## Depth Layer System

Not every agent needs the same depth. This skill defines 4 layers.
Every layer produces an Agent Spec — the difference is completeness.

```
Layer  Sketch ─── 5 questions, quick validation
                     │
Layer  Standard ─── Full Scope Interview (6 domains)
                     │
Layer  Deep ──────── Standard + Ecosystem Map + Dependency Trace
                     │
Layer  Critical ──── Deep + Risk Model + Cost Model + Safety Gates
```

### Layer Selection

Choose the layer based on these signals:

| Signal | Sketch | Standard | Deep | Critical |
|--------|--------|----------|------|----------|
| Add 1-2 capabilities to existing agent | ✅ | — | — | — |
| New agent, single purpose, no external deps | — | ✅ | — | — |
| New agent, 1-2 external integrations | — | ✅ | ✅ | — |
| Agent in multi-agent ecosystem | — | — | ✅ | ✅ |
| Agent handles payment / auth / PII | — | — | — | ✅ |
| Agent deploys to production | — | — | — | ✅ |
| Agent costs significant tokens per call | — | — | — | ✅ |
| Safety or compliance implications | — | — | — | ✅ |

**Default to Standard.** Promote only when explicit signals are present.
Do not promote because of uncertainty alone.

**Escalation rule:** If during a lower-layer interview you discover signals
that warrant a higher layer, promote explicitly:

```
Promotion note:
  Trigger: [what you found]
  Evidence: [where you found it]
  Risk of staying: [why lower layer is unsafe]
```

---

## Layer 1 — Sketch

For small, well-understood changes. The scope is narrow — a new capability,
a prompt adjustment, a tool addition to an existing agent.

### Entry Criteria

- Agent already exists.
- Change is limited to 1-2 capabilities.
- No new integrations, no ecosystem change.

### Process

1. **Intent Check** — Restate the request in one sentence.
2. **5 Quick Questions:**
   - What changes? (capability / tool / prompt / behavior?)
   - Does this change the agent's identity? (name, role, tone?)
   - Does this affect other agents in the ecosystem?
   - Is a new tool or MCP needed?
   - Is there a failure mode if this change goes wrong?
3. **Propose Spec** — Output a condensed spec (1 page).
4. **Confirm** — One-line approval from user.

### Output

A condensed Agent Spec with only changed fields.

---

## Layer 2 — Standard

For new agents and significant changes to existing agents.
The default layer for any agent design task.

### Process

```
1. Intent Gate      → Restate, identify user + goal + constraint
2. Codebase Scan    → Read index.md, opencode.jsonc, AGENTS.md, existing agents
3. Scope Interview  → Full 6-domain deep dive (see below)
4. Propose Spec     → Full Agent Spec (see template)
5. Approval Gate    → User approves before build
```

### Scope Interview (6 Domains)

#### Domain 1: Identity & Purpose

Who is this agent? What is its reason to exist?

- Name — what is the agent called? (propose if unnamed)
- Role — what function does it serve in the system?
- Persona — what communication style? formal / casual / technical / bilingual?
- Context — what must it know before working? (project, repo, env, rules)
- Scope — what is the narrowest useful definition of its job?

#### Domain 2: Capabilities & Boundaries

What can it do? What must it never do?

- Capabilities — list the core actions, ranked by priority.
- Boundaries — explicit list of what it must not do.
- Tools — what tools does it need? (MCPs, APIs, CLI, filesystem)
- Skills — what skills must it load before work?
- Delegation — when should it hand off to another agent? Which one?
- Failure handling — what happens when a capability fails?

#### Domain 3: Input & Output

- Input formats — text, file, command, webhook, voice, structured data?
- Output formats — message, file, API call, action, email, state change?
- Format constraints — JSON schema? Markdown structure? Naming convention?
- Timing — synchronous (wait for response) or async (fire and forget)?
- Error output — how does it report errors? To user? To log? To other agent?

#### Domain 4: Ecosystem

Where does this agent live? Who does it talk to?

- Platform — OpenCode? ZCode? Claude Code? Custom?
- Placement — subagent? plugin? standalone? MCP server?
- Agent team — works alone? reports to supervisor? peers with others?
- Dependencies — env vars? API keys? config files? services? databases?
- Communication — how does it receive work? (user message? agent message? event? trigger?)
- Knowledge — what files/repos/docs must it read before first use?

#### Domain 5: Lifecycle

- Trigger — always active? on-demand? scheduled? event-driven?
- Auto-load — must be loaded automatically or only when called?
- Scope duration — how long does this agent exist? (one session? permanent? until removed?)
- Maintenance — who updates it? what happens when it breaks?
- Memory — does it need persistent state? journal? log?
- Retirement — when and how is this agent removed?

#### Domain 6: Risk & Constraints

- Failure modes — what are the most likely failures? Worst-case scenario?
- Sensitive data — does it handle PII, secrets, credentials, private repos?
- Cost — token budget per session? daily cap? warning threshold?
- Safety — what guardrails are needed? (approval gates, read-only defaults,
  confirmation before destructive actions)
- Testing — how do you verify it works correctly?
- Fallback — what happens if it can't complete its task?

---

## Layer 3 — Deep

For complex agents with multiple integrations, tools, or ecosystem dependencies.

### Additional Process (after Standard)

1. **Ecosystem Map** — Map all agents, tools, MCPs, and services this agent
   interacts with. Use Mermaid if useful.

   ```
   Agent-X
     ├── MCP: Obsidian (read knowledge base)
     ├── MCP: Exa (web search)
     ├── Agent: Steward (for system operations)
     └── Service: OpenAI API (fallback LLM)
   ```

2. **Dependency Trace** — For each dependency:
   - Is it available in all environments?
   - Does it need auth? API key? env var?
   - What happens if it's unavailable?
   - Is there a fallback?

3. **Interaction Design** — For each agent-agent interaction:
   - Who initiates?
   - What data is passed?
   - What format?
   - Error handling?

4. **Failure Mode Analysis** — For each integration:
   - Network failure
   - Auth expiry
   - Rate limiting
   - Data format mismatch
   - Version drift

### Output

Standard Agent Spec + Ecosystem Map + Interaction Contracts + Failure Register

---

## Layer 4 — Critical

For production-grade agents. Adds risk, cost, safety, and compliance layers.

### Additional Process (after Deep)

1. **Risk Assessment** — Per failure mode:
   - Probability (Low/Medium/High)
   - Impact (Low/Medium/High/Critical)
   - Mitigation
   - Detection method

2. **Cost Model** — For each call/session/day:
   - Token budget per interaction
   - Estimated cost per session
   - Monthly projection
   - Cost warning thresholds
   - Cost alert triggers
   - Cache hit target

3. **Safety Gate** — For each high-risk action:
   - Read-only by default?
   - Human approval required? (for what actions?)
   - Audit log needed?
   - Rollback plan?

4. **Compliance Check** — If relevant:
   - Data residency
   - PII handling
   - Retention policy
   - Access control

5. **Approval Workflow** — Define who approves what:
   - Spec approval (must)
   - Cost approval (if > threshold)
   - Safety approval (if high-risk)
   - Pre-deployment review (must)

### Output

Full Agent Spec + Ecosystem Map + Interaction Contracts + Failure Register +
Risk Register + Cost Model + Safety Review + Approval Log

---

## Workflow

```
User request
    │
    ▼
  ┌──────────────────────────────────────┐
  │          1. INTENT GATE              │
  │  Restate request, identify user,     │
  │  goal, constraint. Check codebase    │
  │  for existing context.               │
  └──────────────────────────────────────┘
    │
    ▼
  ┌──────────────────────────────────────┐
  │          2. SELECT LAYER             │
  │  Use signal table to pick            │
  │  Sketch / Standard / Deep / Critical │
  │  Default: Standard.                  │
  │  Write selection + justification.    │
  └──────────────────────────────────────┘
    │
    ▼
  ┌──────────────────────────────────────┐
  │          3. SCOPE INTERVIEW          │
  │  Depth depends on layer:             │
  │  Sketch → 5 questions                │
  │  Standard → 6-domain interview       │
  │  Deep → + Ecosystem map + deps       │
  │  Critical → + Risk + Cost + Safety   │
  │                                      │
  │  Technique: Grill Methodology        │
  │  (see section below)                 │
  └──────────────────────────────────────┘
    │
    ▼
  ┌──────────────────────────────────────┐
  │          4. PROPOSE SPEC             │
  │  Write Agent Spec at layer depth.    │
  │  Present to user.                    │
  └──────────────────────────────────────┘
    │
    ▼
  ┌──────────────────────────────────────┐
  │          5. APPROVAL GATE            │
  │  User reviews spec → approve /       │
  │  revise / reject.                    │
  │  No build until approved.            │
  └──────────────────────────────────────┘
    │
    ▼
  ┌──────────────────────────────────────┐
  │          6. BUILD OR HANDOFF         │
  │  If approved → build OR write        │
  │  handoff notes for next agent.       │
  │  Update index.md + journal.          │
  └──────────────────────────────────────┘
```

---

## Grill Methodology (How to Ask)

Borrowed from `grill-with-docs`. Use these techniques during the Scope Interview.

### T1: Decision Tree Walk

Ask one question at a time. Each question follows from the previous answer.
Do not jump across domains.

```
Identity → Capabilities → I/O → Ecosystem → Lifecycle → Risk
```

Stay in the current domain until it's resolved. Only move to the next domain
when the current one has no more open questions.

### T2: One at a Time

Ask exactly one question. Wait for the answer. Then ask the next.
Exceptions:
- Questions that can be answered from codebase → search first, don't ask.
- Questions about established patterns → check existing agents for precedent.

### T3: Recommend an Answer

Every question includes a recommendation. Give the user something to agree,
reject, or modify.

```
❌ "What personality should this agent have?"
✅ "I suggest a neutral, professional Thai tone —
    formal enough for documentation, relaxed enough for chat.
    Or would you prefer something else?"
```

### T4: Codebase-First

Before asking any question, check if the answer exists in:

- `index.md` — existing agents, system structure
- `opencode.jsonc` — MCPs, plugins, config
- `AGENTS.md` — agent rules
- `CONTEXT.md` — environment, paths
- `PROFILE.md` — user preferences
- `skill-library/` — available skills
- Other agent configs for pattern reference

If found → use it. If not found → report "not found in codebase" → then ask.

### T5: Sharpen Fuzzy Language

When the user uses vague terms, pin them down.

| Vague phrase | Sharpen to |
|-------------|-----------|
| "manage" | read? create? update? delete? moderate? |
| "handle" | process? route? store? forward? |
| "data" | what type? what format? what size? |
| "user" | who exactly? customer? admin? member? |
| "system" | which system? agent? platform? service? |
| "integrate" | API call? webhook? shared DB? event bus? |

### T6: Concrete Scenarios

When discussing behavior, boundaries, or failure modes — propose a concrete
scenario.

```
Question: "What if this agent receives a request it can't fulfill?"
Scenario: "Example: Agent-X only reads files but someone asks it to write.
           Should it: (a) refuse politely, (b) suggest another agent,
           (c) escalate to the user, (d) throw an error?"
```

### T7: Cross-Reference

When the user proposes a behavior or capability — check existing agents
for precedent. If an existing agent does something similar, reference it.

```
User: "This agent should auto-summarize daily."
Cross-ref: "Steward has a daily journal pattern in index.md —
            should we follow that format, or design a new one?"
```

---

## Agent Spec Template

This is the output of every Create Agent session. Fill fields at the
appropriate depth for the selected layer.

```markdown
# Agent Spec: [Name]

## v[0.1.0] — [YYYY-MM-DD]
Layer: [Sketch / Standard / Deep / Critical]
Status: [Proposed / Approved / In Progress / Built]

---

## Identity

- **Name:**
- **Role:**
- **Persona:**
- **Knows context:**
- **Narrowest job:**

---

## Capabilities

| # | Capability | Priority | Notes |
|---|-----------|----------|-------|
| 1 | | High/Med/Low | |
| 2 | | | |

### Boundaries (explicit do-not-do)
- 

### Failure Handling
- 

---

## Tools & MCPs

| Tool/MCP | Purpose | Config | Auth |
|----------|---------|--------|------|
| | | | |

---

## Input / Output

- **Input format:**
- **Output format:**
- **Timing:** [sync / async]
- **Error output:**

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

---

## Lifecycle

- **Trigger:**
- **Auto-load:** [yes / no]
- **Duration:**
- **Maintenance:**
- **Memory / Journal:**
- **Retirement plan:**

---

## Risk & Safety (Layer 3+)

| Failure | Probability | Impact | Mitigation |
|---------|------------|--------|------------|
| | | | |

### Sensitive Data
- 

### Cost Model
- **Per-call budget:**
- **Per-session estimate:**
- **Monthly projection:**
- **Cache hit target:**
- **Alert threshold:**

### Safety Gates
- [ ] Read-only by default?
- [ ] Human approval required?
- [ ] Audit log?
- [ ] Rollback plan?

---

## Approval

| Item | Approved? | Approver | Date |
|------|-----------|----------|------|
| Spec | | | |
| Cost | | | |
| Safety | | | |

---

## Notes

-
```

---

## Checkpoint Gates

These gates must pass before moving to the next phase.

### Intent Gate
- [ ] Request restated
- [ ] User identified
- [ ] Goal clear
- [ ] Constraint noted
- [ ] Codebase scanned for existing context
- [ ] Layer selected with justification

### Scope Gate (after interview)
- [ ] All questions for this layer answered
- [ ] Codebase searched for answers first
- [ ] Every vague term sharpened
- [ ] At least one scenario tested per risk
- [ ] Cross-reference with existing agents done

### Spec Gate (before approval)
- [ ] Agent Spec written at correct layer depth
- [ ] All empty fields explicitly marked TBD or N/A
- [ ] Approval items listed
- [ ] Spec presented to user

### Build Gate
- [ ] Spec approved by user
- [ ] index.md updated
- [ ] Journal updated
- [ ] Handoff notes written (if not building)

---

## When Not To Use This Skill

- Editing an existing agent's prompt without changing its scope or identity.
- Debugging or fixing a broken agent (scope is already defined).
- System administration or DevOps tasks unrelated to agent design.
- Research or exploration without a design intent.

---

## Related

- [`idea-to-architecture-agent`](https://github.com/aetox-skills/idea-to-architecture-agent)
  — scope-first for software architecture, not agent-specific.
- [`senior-architect-agent`](https://github.com/aetox-skills/senior-architect-agent)
  — architecture mapping for existing systems.
- `grill-with-docs` — questioning methodology reference (in skill-library).
- `aetox-skills/token-saver` — RTK protocol for token efficiency.
