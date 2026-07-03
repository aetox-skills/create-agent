---
name: create-agent
description: >-
  Use this skill to design or modify AI agents with a scope-first process.
  Clarifies mission, boundaries, authority, inputs, outputs, risks, metrics,
  tools, tests, and stop conditions before producing an Agent Spec.
---

# Create Agent

## Purpose

Design AI agents with a scope-first discipline. Before writing any prompt,
config, or tool binding, you must pass a structured scope interview. The depth
of the interview depends on the complexity of the agent being designed.

This is **not** a prompt-writing skill. It is a **scope-validation** skill.
The output is a validated **Agent Spec** — a blueprint that another AI or human
can use to build the agent correctly.

Every agent spec defines 8 dimensions:

```
Archetype → Personality → Capabilities → I/O → Ecosystem → Memory → Lifecycle → Risk/Metrics
```

---

## Iron Rules

1. **Scope before build** — No code, no prompt, no tool config until scope is
   validated. Zero exceptions.
2. **Minimum sufficient depth** — Select the lightest layer that can safely
   define the agent. Promote only when explicit signals appear.
3. **Infer first, ask later** — Before asking a question, try to infer the
   answer from context, codebase, existing agents, and what the user already
   said. Only ask about what remains genuinely uncertain.
4. **Codebase-first** — If the answer to a question exists in config, docs,
   or existing agents — go find it. Do not ask.
   **Greenfield exception:** If no codebase exists (no index.md, AGENTS.md,
   opencode.jsonc, or existing agents), skip codebase scan and go directly
   to the interview.
5. **One question at a time** — Ask, wait for answer, then ask the next.
   Do not dump 10 questions at once.
6. **Recommend, don't open-ended** — Every question must include a
   recommended answer. Give something to react to.
7. **State explicit** — Never proceed on implicit understanding.
   If it's not written, it's not agreed.
   **Quick-layer exception:** User confirmation of an inferred spec counts
   as explicit agreement. Document it in one line (`Status: Approved via Quick`).
8. **Stop on signal** — When the user says "enough" or "stop" — stop.
   Do not argue, do not push for more.
9. **Output an Agent Spec** — Every session produces a validated spec, even
   at Quick layer.

---

## Agent Archetype System

Agents follow patterns based on their archetype. Use this table to pick the
right archetype and understand which interview domains to focus on or skip.

### Archetype Catalog

| # | Archetype | Core Purpose | Default Personality | Typical Tools | Default Layer |
|:-:|-----------|-------------|--------------------|--------------|:-------------:|
| 🛠️ | **System** | Automate, config, maintain | Practical, concise, direct | CLI, filesystem, Docker, git | Standard |
| 🎨 | **Creative** | Design, visual, motion | Expressive, detail-oriented | Tailwind, GSAP, image APIs | Standard |
| 💻 | **Developer** | Code, architecture, debug | Technical, precise, thorough | MCPs, linters, test runners | Deep |
| 📝 | **Scribe** | Document, write, organize | Clear, structured, patient | Obsidian, markdown, search | Standard |
| 🔬 | **Specialist** | Deep single domain | Authoritative, meticulous | Domain-specific CLI/MCP | Deep |
| 🎯 | **Orchestrator** | Coordinate, delegate, manage | Systematic, process-driven | Agent list, handoff protocol | Critical |
| 🔍 | **Researcher** | Search, analyze, synthesize | Curious, analytical, thorough | Exa, Firecrawl, web tools | Standard |
| 🎮 | **Game** | Game-specific behavior | Playful, adaptive | Game API, bot framework | Deep |

### Archetype Skip List

Don't waste time on domains this archetype doesn't care about:

| Archetype | Skip These Domains | Focus On |
|-----------|-------------------|----------|
| **System** | I/O, Memory | Capabilities, Ecosystem |
| **Creative** | Risk/Metrics, Lifecycle, Memory | Identity, I/O, Personality |
| **Developer** | Lifecycle, Identity | Capabilities, Ecosystem, I/O |
| **Scribe** | Risk/Metrics, Ecosystem | Identity, I/O, Memory |
| **Specialist** | I/O, Lifecycle | Capabilities, Risk/Metrics |
| **Orchestrator** | — (skip nothing) | Ecosystem, Lifecycle, Risk/Metrics |
| **Researcher** | Lifecycle, Memory, Personality | Capabilities, I/O, Identity |
| **Game** | Ecosystem, Risk/Metrics | Capabilities, I/O, Personality |

### Archetype Selection

When interviewing, recommend an archetype based on the agent's purpose.
If multiple archetypes fit, pick the dominant one and note secondary.

```
Example: "This sounds like a Developer archetype with some Specialist overlap.
         Per skip list, I'll skip Lifecycle and focus on Capabilities + Ecosystem."
```

---

## Depth Layer System

Not every agent needs the same depth. This skill defines **4 layers**.

```
Layer 1 — Quick       (infer → confirm → build, ถามเพิ่มถ้า uncertain)
                       │
Layer 2 — Standard    (Full 8-domain Scope Interview — default)
                       │
Layer 3 — Deep        (Standard + Ecosystem Map + Dependency Trace)
                       │
Layer 4 — Critical    (Deep + Risk Model + Cost Model + Safety Gates)
```

### Layer Selection

| Signal | Quick | Standard | Deep | Critical |
|--------|:-----:|:--------:|:----:|:--------:|
| Agent is new or modified (1-2 caps), scope clear | ✅ | — | — | — |
| Existing agent, small change | ✅ | — | — | — |
| New agent, single purpose, no deps | — | ✅ | — | — |
| New agent, 1-2 external integrations | — | ✅ | ✅ | — |
| Agent in multi-agent ecosystem | — | — | ✅ | ✅ |
| Handles payment / auth / PII | — | — | — | ✅ |
| Deploys to production | — | — | — | ✅ |
| High token cost per call | — | — | — | ✅ |
| Safety or compliance implications | — | — | — | ✅ |
| Archetype = Orchestrator | — | — | — | ✅ |
| Archetype = Specialist | — | — | ✅ | — |

**Default to Standard.** Promote only when explicit signals are present.

**Escalation rule:** If during a lower-layer interview you discover signals
that warrant a higher layer, promote explicitly:

```
Promotion note:
  Trigger: [what you found]
  Evidence: [where you found it]
  Risk of staying: [why lower layer is unsafe]
```

---

## Layer 1 — Quick

For simple agents and small changes. Infer the spec from context, confirm
with the user, build. No full interview, no separate approval gate.

Don't overthink this layer. If uncertain whether it fits Quick, use Standard.

### Entry Criteria

- Agent is new (1-2 capabilities) or existing with small modification.
- No external integrations beyond what already exists.
- No multi-agent coordination.
- No safety, cost, or compliance risk.
- Requirements are clear enough that an 8-domain interview would feel
  like over-engineering.

### Process

```
1. Infer spec from context    ← อ่านจากที่ user บอก + codebase + existing agents
2. Confirm with user          ← เสนอ spec — "ตรงไหนแก้บอก"
   ถ้ามี uncertainty → ถามเพิ่ม 1-2 ข้อ (อย่าถามทั้ง 8 domains)
3. Build                      ← user confirm = approval
```

### Output

A condensed Agent Spec (1-page brief). Skip sections that are obvious.

For **modifications to existing agents**, output only changed fields
(= "diff spec").

### Example

```
User: "อยากได้ agent ที่สรุปเมล์ให้หน่อย"

Infer:
  Archetype: Scribe
  Capability: read email → bullet summary
  Tools: Gmail MCP (มีอยู่แล้ว)
  Risk: ต่ำ, read-only
  I/O: input = email → output = summary markdown
  Ecosystem: อยู่กับ steward

Confirm:
  "Infer มาแบบนี้ — สรุปไทย/อังกฤษ?
   สรุปทุกฉบับหรือเฉพาะหัวข้อ?
   เก็บ log ไหม?"

User: "ไทย, ทุกฉบับ, ไม่ต้อง log"

Build.
  → Spec: condensed, one-liner Status: Approved via Quick
```

---

## Layer 2 — Standard

For new agents and significant changes. The default layer.

### Process

```
1. Intent Gate       → Restate, identify archetype, user + goal + constraint
2. Codebase Scan     → Read index.md, opencode.jsonc, AGENTS.md, existing agents
                       (ถ้าไม่มี → greenfield: skip, ไป interview เลย)
3. Archetype Match   → Select archetype, apply skip list
4. Scope Interview   → Full 8-domain (skipping per archetype skip list)
5. Propose Spec      → Full Agent Spec (see templates/agent-spec.md)
6. Approval Gate     → User approves before build
```

### Scope Interview (8 Domains)

สำหรับ Quick layer: ข้ามทั้งหมด (infer + confirm เอาตั้งแต่ต้น)
สำหรับ Standard+: ใช้ checklist ด้านล่าง แต่ apply Archetype Skip List ก่อน
ถ้า archetype skip domain ไหน — อย่าถาม

#### Domain 1: Identity & Purpose

- **Name** — what is the agent called? (propose if unnamed)
- **Archetype** — which archetype per catalog?
- **Role** — one clear sentence: "This agent [does X] for [user Y] so that [Z]."
- **Persona** —

  | Field | What | Example |
  |-------|------|---------|
  | Tone | formal / casual / technical / playful / authoritative / warm | "technical but approachable" |
  | Example phrase | คำพูดจริงที่ agent พูด | "Let me check the config... Found it. The issue is in line 42." |

  **Always recommend a persona.**
  ```
  "I suggest a technical but approachable tone —
   precise on facts, relaxed in conversation."
  ```

- **Knows context** — what must it know before working?
- **Narrowest job** — what is the smallest useful definition of its role?

#### Domain 2: Capabilities & Boundaries

- **Capabilities** — core actions, ranked by priority.
  Each capability includes:
  - Description (1 sentence)
  - Trigger (how is this capability invoked?)
  - Expected output (what does success look like?)
  - Failure mode (what if it doesn't work?)

- **Boundaries** — explicit list of what it must never do.
  Format each as a **Critical Rule** with scenario:

  ```
  Rule: Never modify files outside the project directory.
  Scenario: User asks "fix my desktop settings" — agent has filesystem access.
  Consequence: Refuse politely, explain scope limit.
  ```

- **Tool Mapping** — for each capability, determine what tools are needed.
  Do this **after** capabilities and boundaries are clear.

  ```
  Capability: "Search documentation"
  ↓
  Search codebase for existing tools → found Exa MCP (web search)
  ↓
  Propose: "Knowledge retrieval via Exa MCP (already configured).
            If Exa is unavailable, fallback to web search via Firecrawl."
  ```

  **Rule:** Propose a **specific tool if one already exists** in the codebase.
  Only propose a **category** when no existing tool fits.

  ```
  ✅ "Uses Exa MCP for web search — already configured"
  ✅ "No existing vector DB found. Needs a semantic search tool
      such as Chroma or Qdrant."
  ❌ "Needs a knowledge retrieval tool" (vague — Exa exists, use it)
  ```

  **Tool categories (reference for gaps):**
  ```
  Knowledge retrieval    → search, vector DBs, document stores, web scraping
  Code execution         → sandboxes, interpreters, compilers, linters
  Communication          → email, messaging, notifications, webhooks
  System operations      → file I/O, process management, network, registry
  Data processing        → ETL, transformation, validation, storage
  Visual/creative        → image generation, layout engines, animation
  Monitoring             → logs, metrics, tracing, alerting
  AI/LLM                 → model APIs, embeddings, RAG pipelines, vector DBs
  Testing                → test runners, coverage, mocking, E2E browsers
  ```

- **Skills** — what skills must it load before work?
- **Delegation** — when should it hand off to another agent? Which one?
- **Failure handling** — what happens when a capability fails?
  (retry? escalate? log? fallback?)

#### Domain 3: Input & Output

- **Input formats** — text, file, command, webhook, voice, structured data?
- **Output formats** — message, file, API call, action, email, state change?
- **Format constraints** — JSON schema? Markdown structure? Naming convention?
- **Deliverables** — what concrete outputs does this agent produce?

  For each deliverable:
  ```
  Deliverable: Bug report
  Format: Markdown
  Template:
    ## Bug: [title]
    ### Environment
    ### Steps to reproduce
    ### Expected vs Actual
    ### Severity
    ### Suggested fix
  ```

- **Timing** — synchronous (wait for response) or async (fire and forget)?
- **Error output** — how does it report errors?

#### Domain 4: Ecosystem

- **Platform** — OpenCode? ZCode? Claude Code? Custom?
- **Placement** — subagent? plugin? standalone? MCP server?
- **Agent team** — works alone? reports to supervisor? peers with others?
- **Dependencies** — env vars? API keys? config files? services? databases?
- **Communication** — how does it receive work?
- **Knowledge** — what files/repos/docs must it read before first use?

#### Domain 5: Memory & Learning

- **Session memory** — what must it remember during one conversation?
- **Persistent memory** — what crosses sessions?
- **Knowledge accumulation** — what should it learn and keep?
- **Forgetting** — what should it explicitly NOT remember?

#### Domain 6: Lifecycle

- **Trigger** — always active? on-demand? scheduled? event-driven?
- **Auto-load** — must be loaded automatically or only when called?
- **Scope duration** — how long does this agent exist?
- **Maintenance** — who updates it? what happens when it breaks?
- **Retirement** — when and how is this agent removed?

#### Domain 7: Risk & Constraints

- **Failure modes** — what are the most likely failures?
- **Sensitive data** — does it handle PII, secrets, credentials?
- **Safety guardrails** — what protection layers are needed?
- **Testing** — how do you verify it works correctly?
- **Fallback** — what happens if it can't complete its task?

#### Domain 8: Success Metrics

How do you know this agent is working well?

At least **1 metric** (not mandatory to have 3). Mix types as needed:

| Metric Type | Examples |
|-------------|----------|
| **Accuracy** | Answer correctness %, error rate, hallucination rate |
| **Speed** | Response time, time-per-task, throughput |
| **Efficiency** | Token cost per task, cache hit rate, retry rate |
| **Quality** | User approval %, rework rate, first-attempt success |
| **Business** | Tasks completed, issues resolved, features shipped |

Format:
```
Metric: First-attempt success rate
Target: >80%
Measure: (tasks passed QA on first try / total tasks) × 100
When: Weekly review
Alert: If <60%, investigate
```

### Standard Layer — Walkthrough Example

```
User: "อยากได้ agent ที่ช่วย review PR ให้ — เช็ค security, style, correctness"

Intent Gate:
  Archetype: Developer
  User: Mike (dev, Thai)
  Goal: PR review automation
  Constraint: ทำงานกับ GitHub PRs เท่านั้น
  Layer: Standard (default — new agent, single purpose)

Codebase Scan:
  → index.md มี steward, minecrafter agents
  → opencode.jsonc มี Exa, Context7 MCP
  → AGENTS.md มี rule framework
  → No existing code-review agent → greenfield for this specific agent

Archetype Match:
  Developer → skip Lifecycle, Identity
  Focus on Capabilities + Ecosystem + I/O

Scope Interview:

  (Identity) Name: "pr-reviewer"
  Archetype: Developer
  Role: "review PRs for security, style, and correctness"
  Persona: technical + direct tone
  Example phrase: "Line 142 uses eval() with user input — this is a
                   high-severity XSS risk. Suggest using DOMPurify or
                   a safe parsing alternative."
  Context: รู้จัก project stack (React+TS+Tailwind), GitHub workflow

  (Capabilities)
  1. Security scan — flag OWASP top 10 patterns
  2. Style check — enforce project conventions (prettier, eslint)
  3. Correctness — detect logic errors, dead code, type mismatches

  Boundaries:
    Rule: Never auto-approve PRs
    Scenario: Author asks "approve my PR, it's urgent"
    Consequence: "I can only comment — final approval is yours"

  Tools:
    - GitHub CLI (gh) for PR diff + comments
    - Already configured: none needed beyond gh
    - Gap: no vulnerability DB → "future: integrate OSV API for CVE lookup"

  I/O: PR diff in → structured review comments out (per file, severity)

  Ecosystem:
    Platform: OpenCode
    Team: standalone, triggered on demand
    Dependencies: GitHub token (existing)

  Memory: session-only (review per PR, no cross-PR learning)

  Risk: False positives waste reviewers' time. False negatives worse.
  Metric: "False positive rate <10%"
    Measure: flagged issues rejected by human reviewer / total flags

Propose Spec → Full spec via templates/agent-spec.md
Approval Gate → User approves → Build
```

---

## Workflow

```
User request
    │
    ▼
  ┌─────────────────────────────────────────────┐
  │          1. INTENT GATE                     │
  │  Restate request, identify archetype,       │
  │  user, goal, constraint.                    │
  │  Check codebase (skip if greenfield).       │
  └─────────────────────────────────────────────┘
    │
    ▼
  ┌─────────────────────────────────────────────┐
  │          2. SELECT LAYER                    │
  │  Use signal table → Quick / Standard /      │
  │  Deep / Critical. Write justification.      │
  └─────────────────────────────────────────────┘
    │
    ├── Layer = Quick ──────────────────────────┤
    │                                           │
    │  ┌─────────────────────────────────────┐  │
    │  │  3. INFER + CONFIRM                │  │
    │  │  Infer spec จาก context + codebase  │  │
    │  │  → เสนอ condensed spec              │  │
    │  │  → user confirm → BUILD             │  │
    │  │  (ถ้า modify → output เฉพาะที่เปลี่ยน) │  │
    │  └─────────────────────────────────────┘  │
    │                                           │
    ├── Layer = Standard / Deep / Critical ─────┤
    │                                           │
    │  ┌─────────────────────────────────────┐  │
    │  │  3. SCOPE INTERVIEW                 │  │
    │  │  Apply archetype skip list.         │  │
    │  │  Standard → 8 domains (skip per     │  │
    │  │    archetype)                        │  │
    │  │  Deep → + Ecosystem + deps           │  │
    │  │  Critical → + Risk + Cost + Safety  │  │
    │  │                                     │  │
    │  │  Infer-first: only uncertain Qs     │  │
    │  └─────────────────────────────────────┘  │
    │                                           │
    │  ┌─────────────────────────────────────┐  │
    │  │  4. PROPOSE SPEC                    │  │
    │  │  Full Agent Spec (templates/agent-  │  │
    │  │  spec.md). Present to user.         │  │
    │  └─────────────────────────────────────┘  │
    │                                           │
    │  ┌─────────────────────────────────────┐  │
    │  │  5. APPROVAL GATE                   │  │
    │  │  User approves / revise / reject.   │  │
    │  │  No build until approved.           │  │
    │  └─────────────────────────────────────┘  │
    │                                           │
    └───────────────────────────────────────────┘
    │
    ▼
  ┌─────────────────────────────────────────────┐
  │          6. BUILD OR HANDOFF                │
  │  If approved → build OR write handoff       │
  │  notes for next agent.                      │
  │  Update index.md + journal.                 │
  └─────────────────────────────────────────────┘
```

---

## Questioning Technique

### T0: Infer-First (Primary)

**Before asking anything, try to fill the spec from what you already know.**
Then ask only the gaps.

```
User: "อยากได้ agent ที่สรุปเมล์หน่อย"

Infer (จาก context + codebase):
  Archetype: Scribe
  Capability: read mail → bullet summary
  Tools: Gmail MCP (มีอยู่แล้ว)
  Risk: ต่ำ, read-only
  Ecosystem: อยู่กับ steward

Ask (เฉพาะที่ยัง uncertain):
  "สรุปภาษาไทยหรืออังกฤษ? สรุปทุกฉบับหรือเฉพาะหัวข้อ?"
```

### T1: Decision Tree Walk

เมื่อต้องถาม — ถามทีละขั้น อย่ากระโดดข้าม domain

```
Identity → Personality → Capabilities → I/O → Ecosystem → Memory → Lifecycle → Risk/Metrics
```

### T2: Recommend an Answer

ทุกคำถามควรมี recommendation — อย่าถามปลายเปิด

```
❌ "personality แบบไหน?"
✅ "แนะนำ tone กลางๆ ทางการ — หรืออยากได้แนวอื่น?"
```

---

## Agent Test Plan

Every agent capability needs a test prompt. Behavioral validation:
"Does the agent do what we designed it to do?"

### When to Create

Standard layer and above. At Quick layer, skip tests for unchanged caps.

### Test Prompt Format

```
Capability: [name]
Test prompt: "[exact prompt to give the agent]"
Expected output: "[what a correct response looks like]"
Edge cases:
  - [edge case 1] → [expected handling]
  - [edge case 2] → [expected handling]
```

### Example

```
Capability: Search knowledge base
Test prompt: "หาข้อมูลเกี่ยวกับ Docker networking"
Expected output: สรุปสั้นๆ 1-3 ข้อ + แหล่งที่มา
Edge cases:
  - ไม่พบข้อมูล → "ไม่พบข้อมูลในฐานความรู้ แนะนำให้ค้นจากภายนอก"
  - คำค้นกำกวม → ถามให้ชัดก่อน
  - ขอข้อมูลที่เป็นความลับ → "ขออภัย ข้อมูลนี้ไม่สามารถเปิดเผยได้"
```

### Test Plan Placement

```
Spec approved → Build agent → Run test prompts
                   │              │
                   │         ┌─────┴──────┐
                   │         ▼            ▼
                   │       PASS          FAIL
                   │         │            │
                   │    ┌────┘            ▼
                   │    │         Iterate & fix
                   ▼    ▼         → rerun → pass → done
              Deploy / Handoff
```

---

## Agent Iteration Loop

### The Loop

```
Build  →  Run test prompts  →  Review results
  ↑                              │
  │                         ┌────┴────┐
  │                         ▼         ▼
  │                      PASS?      FAIL?
  │                         │         │
  │                    ┌────┘         ▼
  │                    │      Identify gap
  │                    │         │
  │                    │    ┌─────┴──────┐
  │                    │    ▼            ▼
  │                    │  Scope gap?  Behavior gap?
  │                    │               │
  │                    │    Redesign   Fix instruction
  │                    │    spec       / rules
  │                    └────┴────────────┘
  │                              │
  └──────────────────────────────┘
        (loop until stable)
```

### When to Stop

| Signal | Action |
|--------|--------|
| All test prompts pass | ✅ Deploy |
| Critical path passes, minor edge cases fail | ⚠️ Log issues, deploy |
| Same failure after 3 redesigns | 🛑 Escalate — may need scope change |
| User says "enough" | 🛑 Stop |

---

## Checkpoint Gates

### Intent Gate
- [ ] Request restated
- [ ] Archetype identified
- [ ] User identified
- [ ] Goal clear
- [ ] Constraint noted
- [ ] Codebase scanned (or greenfield acknowledged)
- [ ] Layer selected with justification

### Spec Gate (before build)
- [ ] Agent Spec written at correct layer depth
- [ ] Persona defined with example
- [ ] Deliverables listed
- [ ] All empty fields marked TBD or N/A
- [ ] Approval items listed
- [ ] Spec presented to user

---

## When Not To Use This Skill

- Editing an existing agent's prompt without changing its scope or identity.
- Debugging or fixing a broken agent (scope is already defined).
- System administration or DevOps tasks unrelated to agent design.
- Research or exploration without a design intent.

---

## Related

- [`templates/agent-spec.md`](templates/agent-spec.md) — output template (single source of truth)
- [`references/deep-critical.md`](references/deep-critical.md) — Deep & Critical layer details
- [`idea-to-architecture-agent`](https://github.com/aetox-skills/idea-to-architecture-agent)
- [`senior-architect-agent`](https://github.com/aetox-skills/senior-architect-agent)
- [`Aetox-Agents-Team`](https://github.com/aetox-skills/Aetox-Agents-Team)
- [`Agency-Agents`](https://github.com/msitarzewski/agency-agents)
