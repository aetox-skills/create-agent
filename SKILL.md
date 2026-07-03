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
5. **One question at a time** — Ask, wait for answer, then ask the next.
   Do not dump 10 questions at once.
6. **Recommend, don't open-ended** — Every question must include a
   recommended answer. Give something to react to.
7. **State explicit** — Never proceed on implicit understanding.
   If it's not written, it's not agreed.
8. **Stop on signal** — When the user says "enough" or "stop" — stop.
   Do not argue, do not push for more.
9. **Output an Agent Spec** — Every session produces a validated spec, even
    at Turbo or Sketch layer.

---

## Agent Anatomy (Progressive Disclosure)

Every agent has a layered structure. Not all layers need to be filled at once,
but understanding the anatomy helps scope the design correctly.

```
agent/
├── Level 1: Identity Layer       ← always visible (~50 words)
│   ├── name
│   ├── description (when to trigger, what it does)
│   └── model / platform hint
│
├── Level 2: Instruction Layer    ← loaded when agent activates
│   ├── role & personality
│   ├── capabilities & boundaries
│   ├── workflow & rules
│   └── communication style
│
└── Level 3: Resource Layer       ← loaded on demand
    ├── knowledge & references    (docs, specs, past decisions)
    ├── tool categories           (types of tools, not specific tools)
    ├── skill references          (pointers to reusable skills)
    └── test prompts & evals      (how to verify it works)
```

### Level 1 — Identity Layer
This is the **frontmatter**. It determines when the agent is summoned.
Must answer: "What is this agent, and when should it activate?"

Keep it tight — 1-2 sentences. Include trigger conditions.
```
name: code-reviewer
description: >-
  Reviews pull requests for security, style, and correctness.
  Use when a PR is opened, or when asked to review code changes.
  Do not use for architecture design or system planning.
```

### Level 2 — Instruction Layer
This is the **body** of the agent. It defines how the agent behaves.
Includes personality, capabilities, rules, workflow, output format.

This is the core of the `create-agent` Scope Interview output.

### Level 3 — Resource Layer
Everything else the agent might need, loaded on demand.

**Knowledge & References:**
- Domain documentation, past decisions, project conventions
- Glossary of terms, common patterns, anti-patterns
- Links to external standards or APIs

**Tool Categories Reference (ดูเพิ่มที่ Domain 2 — Tool Mapping สำหรับ process จริง):**

A complete list of tool categories is in Domain 2 (Capabilities & Boundaries).
Use that section for the actual tool mapping step. This is just a reminder
that tool categories exist as Level 3 resources.

Key principle: propose **categories** not specific tools.
See Domain 2 for examples and process.

**Skill References:**
- Pointers to reusable skills in `skill-library/` that this agent should load
- Not inline — just references with when-to-use notes

**Test Prompts & Evals:**
- See Agent Test Plan section below

---

## Agent Archetype System

Agents are not all the same. Their structure, personality, tools, and
deliverables follow patterns based on their archetype.

Use this table to understand what type of agent you are designing.
This informs the Scope Interview — each archetype has different weight
per domain.

### Archetype Catalog

| # | Archetype | Core Purpose | Default Personality | Typical Tools | Layer |
|:-:|-----------|-------------|--------------------|--------------|-------|
| 🛠️ | **System** | Automate, config, maintain | Practical, concise, direct | CLI, filesystem, Docker, git | Standard |
| 🎨 | **Creative** | Design, visual, motion | Expressive, detail-oriented | Tailwind, GSAP, image APIs | Standard |
| 💻 | **Developer** | Code, architecture, debug | Technical, precise, thorough | MCPs, linters, test runners | Deep |
| 📝 | **Scribe** | Document, write, organize | Clear, structured, patient | Obsidian, markdown, search | Standard |
| 🔬 | **Specialist** | Deep single domain | Authoritative, meticulous | Domain-specific CLI/MCP | Deep |
| 🎯 | **Orchestrator** | Coordinate, delegate, manage | Systematic, process-driven | Agent list, handoff protocol | Critical |
| 🔍 | **Researcher** | Search, analyze, synthesize | Curious, analytical, thorough | Exa, Firecrawl, web tools | Standard |
| 🎮 | **Game** | Game-specific behavior | Playful, adaptive | Game API, bot framework | Deep |

### How Archetypes Affect the Interview

| Interview Domain | System | Creative | Developer | Scribe | Specialist | Orchestrator | Researcher | Game |
|-----------------|--------|----------|-----------|--------|------------|--------------|------------|------|
| **Identity** | Medium | **High** | Medium | Medium | **High** | Medium | Low | **High** |
| **Capabilities** | **High** | Medium | **High** | Medium | **High** | Medium | **High** | Medium |
| **I/O** | Low | **High** | Medium | **High** | Low | Low | Medium | Medium |
| **Ecosystem** | **High** | Low | **High** | Low | Medium | **High** | Low | Low |
| **Lifecycle** | Medium | Low | Medium | Medium | Low | **High** | Low | Medium |
| **Risk/Metrics** | Medium | Low | Medium | Low | **High** | **High** | Low | Low |

High = spend more time on this domain | Low = can be brief

### Archetype Selection

When interviewing, recommend an archetype based on the agent's purpose.
If multiple archetypes fit, pick the dominant one and note secondary.

```
Example: "This sounds like a Developer archetype with some Specialist overlap.
         I'll prioritize Capabilities and Ecosystem in the interview."
```

---

## Depth Layer System

Not every agent needs the same depth. This skill defines 4 layers.
Every layer produces an Agent Spec — the difference is completeness.

```
Layer  Sketch ─── 5 questions, quick validation
                     │
Layer  Standard ─── Full Scope Interview (8 domains)
                     │
Layer  Deep ──────── Standard + Ecosystem Map + Dependency Trace
                     │
Layer  Critical ──── Deep + Risk Model + Cost Model + Safety Gates
```

### Layer Selection

Choose the layer based on the archetype and these signals:

| Signal | Turbo | Sketch | Standard | Deep | Critical |
|--------|:----:|:------:|:--------:|:----:|:--------:|
| Simple agent, 1 capability, clear scope | ✅ | — | — | — | — |
| Add 1-2 capabilities to existing agent | — | ✅ | — | — | — |
| New agent, single purpose, no deps | — | — | ✅ | — | — |
| New agent, 1-2 external integrations | — | — | ✅ | ✅ | — |
| Agent in multi-agent ecosystem | — | — | — | ✅ | ✅ |
| Agent handles payment / auth / PII | — | — | — | — | ✅ |
| Agent deploys to production | — | — | — | — | ✅ |
| Agent costs significant tokens per call | — | — | — | — | ✅ |
| Safety or compliance implications | — | — | — | — | ✅ |
| Archetype = Orchestrator | — | — | — | — | ✅ |
| Archetype = Specialist | — | — | — | ✅ | — |

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

## Layer 0 — Turbo

For dead-simple agents. User says "want agent that does X" and the scope is
obvious. No interview needed — just infer, confirm, build.

Don't overthink this layer. If you're unsure whether it fits, use Sketch.

### Entry Criteria

- Agent is new or trivially modified (1-2 capabilities).
- No external integrations beyond what already exists.
- No multi-agent coordination.
- No safety, cost, or compliance risk.
- The requirements are clear enough that an 8-domain interview would feel
  like over-engineering.

### Process

```
1. Infer spec from context    ← อ่านจากที่ user บอก + codebase + existing agents
2. Confirm with user          ← "นี่คือ spec ที่ผม infer — ตรงไหนแก้บอก"
3. Build                      ← ไม่ต้องรอ approval
```

### Output

A condensed Agent Spec. Skip sections that are obvious.

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
```

---

## Layer 1 — Sketch

For small, well-understood changes. The scope is narrow — a new capability,
a prompt adjustment, a tool addition to an existing agent.

### Entry Criteria

- Agent already exists.
- Change is limited to 1-2 capabilities.
- No new integrations, no ecosystem change.
- Archetype does not change.

### Process

1. **Intent Check** — Restate the request in one sentence.
2. **5 Quick Questions:**
   - What changes? (capability / tool / prompt / behavior?)
   - Does this change the agent's identity or personality?
   - Does this affect other agents in the ecosystem?
   - Is a new tool or MCP needed?
   - Is there a failure mode if this change goes wrong?
3. **Propose Spec** — Output a condensed spec with only changed fields.
4. **Confirm** — One-line approval from user.

### Output

A condensed Agent Spec with only changed fields.

---

## Layer 2 — Standard

For new agents and significant changes to existing agents.
The default layer for any agent design task.

### Process

```
1. Intent Gate       → Restate, identify archetype, user + goal + constraint
2. Codebase Scan     → Read index.md, opencode.jsonc, AGENTS.md, existing agents
3. Archetype Match   → Select archetype, set domain weights
4. Scope Interview   → Full 8-domain deep dive (see below)
5. Propose Spec      → Full Agent Spec (see template)
6. Approval Gate     → User approves before build
```

### Scope Interview (8 Domains)

#### Domain 1: Identity & Purpose

Who is this agent? What is its reason to exist?

- **Name** — what is the agent called? (propose if unnamed)
- **Archetype** — which archetype? (System/Creative/Developer/Scribe/...)
- **Role** — what function does it serve in the system?
  Use one clear sentence: "This agent [does X] for [user Y] so that [outcome Z]."
- **Persona** — what is its character?

  | Dimension | Options |
  |-----------|---------|
  | Tone | formal / casual / technical / playful / authoritative / warm |
  | Language | Thai / English / bilingual / code-only |
  | Verbosity | concise / balanced / detailed |
  | Style | direct / Socratic / narrative / bullet-point |
  | Vibe (1-2 words) | e.g. "pragmatic realist", "patient teacher", "sharp critic" |

  **Always recommend a persona with examples.**
  ```
  "I suggest a technical but approachable tone —
   precise on facts, relaxed in conversation.
   Example: 'Let me check the config... Found it.
   The issue is in line 42 — missing env var.'"
  ```

- **Context** — what must it know before working?
  (project, repo, env, rules, conventions)
- **Narrowest job** — what is the smallest useful definition of its role?

#### Domain 2: Capabilities & Boundaries

What can it do? What must it never do?

- **Capabilities** — list the core actions, ranked by priority.
  Each capability should include:
  - Description (1 sentence)
  - Trigger (how is this capability invoked?)
  - Expected output (what does success look like?)
  - Failure mode (what if it doesn't work?)

- **Boundaries** — explicit list of what it must never do.
  Format each as a **Critical Rule** with scenario:

  ```
  Rule: [statement]
  Scenario: [concrete example of when this rule applies]
  Consequence: [what happens if broken]
  ```

  ```
  Rule: Never modify files outside the project directory.
  Scenario: User asks "fix my desktop settings" — agent has filesystem access.
  Consequence: Refuse politely, explain scope limit.
  ```

- **Tool Mapping** — for each capability, determine what **categories** of tools
  are needed. Do this **after** capabilities and boundaries are clear.

  ```
  Capability: "Search documentation"
  ↓
  Needs: knowledge retrieval tool (category)
  ↓
  Search codebase: what search/retrieval tools already exist?
  → Found: Exa MCP (web search), Obsidian (local docs)
  → Gap: no vector DB for semantic code search
  ↓
  Propose: "Knowledge retrieval: uses Exa + Obsidian for now.
            Consider adding a vector DB (e.g., Chroma/Qdrant)
            if semantic code search becomes a frequent need."
  ```

  **Rule:** Propose tool **categories**, not specific tools.
  If you know existing tools that fit the category, mention them as examples,
  but frame them as "currently available options" not "must use".

  ```
  ✅ "Needs a knowledge retrieval tool — Exa MCP is already configured
     and could serve this purpose."
  ✅ "Needs code execution — consider a sandbox environment.
     Docker is available in the current setup."
  ❌ "Needs Exa MCP" (too specific, no rationale)
  ❌ "Needs Docker" (too specific, doesn't explain why)
  ```

  **Tool categories reference (direction only, not exhaustive):**
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

  Choose only the categories relevant to this agent. Do not list them all.

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
  To user? To log? To other agent?

#### Domain 4: Ecosystem

Where does this agent live? Who does it talk to?

- **Platform** — OpenCode? ZCode? Claude Code? Custom?
- **Placement** — subagent? plugin? standalone? MCP server?
- **Agent team** — works alone? reports to supervisor? peers with others?
- **Dependencies** — env vars? API keys? config files? services? databases?
- **Communication** — how does it receive work?
  (user message? agent message? event? trigger?)
- **Knowledge** — what files/repos/docs must it read before first use?

#### Domain 5: Memory & Learning

What should this agent remember? How does it learn over time?

- **Session memory** — what must it remember during one conversation?
  ```
  Conversation state: current task, files touched, decisions made.
  ```

- **Persistent memory** — what crosses sessions?
  ```
  Cross-session: user preferences, project conventions,
  past decisions, error patterns.
  ```

- **Knowledge accumulation** — what should it learn and keep?
  ```
  Learn: successful fix patterns, frequently used tools,
  user's shorthand terms, workflow preferences.
  Storage: journal files, memory files, index updates.
  ```

- **Forgetting** — what should it explicitly NOT remember?
  ```
  Forget: temporary file paths, one-time credentials,
  debug output from resolved issues.
  ```

#### Domain 6: Lifecycle

- **Trigger** — always active? on-demand? scheduled? event-driven?
- **Auto-load** — must be loaded automatically or only when called?
- **Scope duration** — how long does this agent exist?
  (one session? permanent? until removed?)
- **Maintenance** — who updates it? what happens when it breaks?
- **Retirement** — when and how is this agent removed?

#### Domain 7: Risk & Constraints

- **Failure modes** — what are the most likely failures?
  Worst-case scenario?
- **Sensitive data** — does it handle PII, secrets, credentials, private repos?
- **Safety guardrails** — what protection layers are needed?
  (approval gates, read-only defaults, confirmation before destructive actions)
- **Testing** — how do you verify it works correctly?
- **Fallback** — what happens if it can't complete its task?

#### Domain 8: Success Metrics

How do you know this agent is working well?

Define at least 3 metrics. Mix types:

| Metric Type | Examples |
|-------------|----------|
| **Accuracy** | Answer correctness %, error rate, hallucination rate |
| **Speed** | Response time, time-per-task, throughput |
| **Efficiency** | Token cost per task, cache hit rate, retry rate |
| **Quality** | User approval %, rework rate, first-attempt success |
| **Business** | Tasks completed, issues resolved, features shipped |

For each metric:
```
Metric: First-attempt success rate
Target: >80%
Measure: (tasks passed QA on first try / total tasks) × 100
When: Weekly review
Alert: If <60%, investigate root cause
```

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

5. **Learning Path** — Define how this agent learns over time:
   - What patterns should it track?
   - Where does it store learnings?
   - How does it surface improvements?

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
  ┌─────────────────────────────────────────────┐
  │          1. INTENT GATE                     │
  │  Restate request, identify archetype,       │
  │  user, goal, constraint. Check codebase.    │
  └─────────────────────────────────────────────┘
    │
    ▼
  ┌─────────────────────────────────────────────┐
  │          2. SELECT ARCHETYPE + LAYER        │
  │  Use archetype catalog → pick archetype     │
  │  Use signal table → pick Turbo / Sketch /   │
  │  Standard / Deep / Critical.                │
  │  Write justification.                       │
  └─────────────────────────────────────────────┘
    │
    ├── Layer = Turbo ──────────────────────────┤
    │                                           │
    │  ┌─────────────────────────────────────┐  │
    │  │  3. INFER + CONFIRM                │  │
    │  │  Infer spec จาก context + codebase  │  │
    │  │  → เสนอ spec → user confirm        │  │
    │  │  → BUILD (no approval gate)        │  │
    │  └─────────────────────────────────────┘  │
    │                                           │
    ├── Layer = Sketch / Standard / Deep / Critical ─┤
    │                                               │
    │  ┌─────────────────────────────────────────┐  │
    │  │  3. SCOPE INTERVIEW                     │  │
    │  │  Depth depends on layer:                │  │
    │  │  Sketch → infer + 5 questions           │  │
    │  │  Standard → 8-domain interview          │  │
    │  │  Deep → + Ecosystem + deps + learning   │  │
    │  │  Critical → + Risk + Cost + Safety      │  │
    │  │                                         │  │
    │  │  Infer-first: fill what you can before  │  │
    │  │  asking. Only ask about uncertainty.    │  │
    │  └─────────────────────────────────────────┘  │
    │                                               │
    │  ┌─────────────────────────────────────────┐  │
    │  │  4. PROPOSE SPEC                        │  │
    │  │  Write Agent Spec at layer depth.       │  │
    │  │  Present to user.                       │  │
    │  └─────────────────────────────────────────┘  │
    │                                               │
    │  ┌─────────────────────────────────────────┐  │
    │  │  5. APPROVAL GATE                       │  │
    │  │  User reviews → approve / revise /      │  │
    │  │  reject. No build until approved.       │  │
    │  └─────────────────────────────────────────┘  │
    │                                               │
    └───────────────────────────────────────────────┘
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

## Agent Test Plan

Every agent capability needs a test prompt. This is not unit testing —
it's behavioral validation: "Does the agent do what we designed it to do?"

### When to Create

Add test plans at **Standard layer and above**. At Sketch layer, skip tests
for unchanged capabilities.

### Test Prompt Format

For each capability, define:

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
  - คำค้นกำกวม → ถามให้ชัดก่อน ("Docker networking — หมายถึง network driver?
    overlay? หรือปัญหาการเชื่อมต่อ?")
  - ขอข้อมูลที่เป็นความลับ → "ขออภัย ข้อมูลนี้ไม่สามารถเปิดเผยได้"
```

### Test Plan Placement

```
Create Agent session
        │
        ▼
  Spec approved → Build agent → Run test prompts
        │                         │
        │                    ┌─────┴──────┐
        │                    ▼            ▼
        │                 PASS          FAIL
        │                    │            │
        │              ┌─────┘            ▼
        │              │         Iterate & fix
        │              │         → rerun test
        │              │         → pass → done
        ▼              ▼
   Deploy / Handoff
```

### What to Test

| Test Type | What | When |
|-----------|------|------|
| **Happy path** | Capability works as designed | Every capability |
| **Boundary** | Agent refuses out-of-scope requests | Every Critical Rule |
| **Failure** | Tool unavailable, bad input, timeout | Layer 3+ |
| **Edge case** | Unusual but realistic input | Layer 3+ |
| **Multi-turn** | Conversation context maintained | Layer 4+ |

---

## Agent Iteration Loop

An agent is not done after the first build. It improves through iteration.

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
  │                    │  (missing     (wrong output,
  │                    │   capability)  bad tone, etc.)
  │                    │    │            │
  │                    │    ▼            ▼
  │                    │  Redesign     Fix instruction
  │                    │  spec         / rules
  │                    │    │            │
  │                    └────┴────────────┘
  │                              │
  └──────────────────────────────┘
        (loop until stable)
```

### When to Stop Iterating

| Signal | Action |
|--------|--------|
| All test prompts pass | ✅ Deploy |
| Critical path passes, minor edge cases fail | ⚠️ Log known issues, deploy |
| Same failure persists after 3 redesigns | 🛑 Escalate — may need scope change |
| User says "enough" | 🛑 Stop |

### Iteration Documentation

Each iteration should be documented:

```
## Iteration 2 — 2026-07-03
What changed: Added retry logic for API failure
Before: API timeout → silent failure
After: API timeout → retry 3x → log → escalate
Test results: 6/6 pass (was 4/6)
Known issues: Rate limit detection not tested — needs live API
```

Documentation lives in the agent's journal or a companion log file.
Keep it light — bullet points, not essays.

---

## Questioning Technique

Use these techniques when you need to ask during the Scope Interview.
The goal is **clarify scope**, not grill the user. Infer first, ask only
what remains genuinely uncertain.

### T0: Infer-First (Primary)

**Before asking anything, try to fill the spec from what you already know.**
Then ask only the gaps.

```
User: "อยากได้ agent ที่สรุปเมล์หน่อย"

Infer (จาก context + codebase):
  Archetype: Scribe (document/summary)
  Capability: read mail → bullet summary
  Tools: Gmail MCP (มีอยู่แล้วใน opencode.jsonc)
  Risk: ต่ำ, read-only
  Ecosystem: อยู่กับ steward

Ask (เฉพาะที่ยัง uncertain):
  "สรุปภาษาไทยหรืออังกฤษ? สรุปทุกฉบับหรือเฉพาะหัวข้อ?"
```

**At Turbo and Sketch layers,** infer the entire spec and just confirm.
No need to walk through domains.

### T1: Decision Tree Walk

เมื่อต้องถามจริงๆ — ถามทีละขั้น อย่ากระโดดข้าม domain

```
Identity → Archetype → Personality → Capabilities → I/O → Ecosystem → Memory → Lifecycle → Risk/Metrics
```

### T2: Recommend an Answer

ทุกคำถามควรมี recommendation ให้ user react — อย่าถามปลายเปิด

```
❌ "personality แบบไหน?"
✅ "แนะนำ tone กลางๆ ทางการ — หรืออยากได้แนวอื่น?"
```

### T3: Sharpen Fuzzy Language

เวลา user พูดคำคลุมเครือ — ถามให้ชัด

| คำคลุมเครือ | ให้ชัดเป็น |
|-------------|-----------|
| "manage" | read? create? update? delete? |
| "handle" | process? route? store? forward? |
| "user" | customer? admin? member? |
| "remember" | session memory? persistent? |

### T4: Concrete Scenarios

เมื่อต้อง discuss behavior หรือ failure mode — ยก scenario ให้เห็นภาพ

```
"ถ้า agent รับ request ที่ทำไม่ได้ — ควรปฏิเสธ? แนะนำ agent อื่น? หรือส่งต่อไป?"
```

### T5: Cross-Reference

เทียบกับ agent ที่มีอยู่ก่อน — มีใครทำ类似 นี้แล้วหรือยัง?

```
User: "อยากให้ agent สรุป daily auto"
Cross-ref: "Steward มี daily journal pattern อยู่แล้วใน index.md —
            ใช้ format เดียวกัน หรือออกแบบใหม่?"
```

---

## Agent Spec Template

This is the output of every Create Agent session. Fill fields at the
appropriate depth for the selected layer.

```markdown
# Agent Spec: [Name]

## v[0.1.0] — [YYYY-MM-DD]
Layer: [Sketch / Standard / Deep / Critical]
Archetype: [System / Creative / Developer / Scribe / Specialist / Orchestrator / Researcher / Game]
Status: [Proposed / Approved / In Progress / Built]

---

## Identity

- **Name:**
- **Archetype:**
- **Role:** This agent [does X] for [user Y] so that [outcome Z].
- **Persona:**

  | Dimension | Value |
  |-----------|-------|
  | Tone | |
  | Language | |
  | Verbosity | |
  | Style | |
  | Vibe | |

  **Example phrases:**
  - [context]: "[example phrase]"
  - [context]: "[example phrase]"

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
| | | |

### Tools & MCPs

| Tool/MCP | Purpose | Config | Auth |
|----------|---------|--------|------|
| | | | |

### Delegation

| When | To Whom | What Passed |
|------|---------|-------------|
| | | |

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

| Metric | Type | Target | Measure | Alert If |
|--------|------|--------|---------|----------|
| | | | | |
| | | | | |
| | | | | |

### Cost Model (Layer 4+)
- **Per-call budget:**
- **Per-session estimate:**
- **Monthly projection:**
- **Cache hit target:**
- **Alert threshold:**

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
- [ ] Archetype identified
- [ ] User identified
- [ ] Goal clear
- [ ] Constraint noted
- [ ] Codebase scanned for existing context
- [ ] Layer selected with justification

### Scope Gate (after interview)
- [ ] All 8 domains covered at correct layer depth
- [ ] Codebase searched for answers first
- [ ] Persona defined with examples
- [ ] Every vague term sharpened
- [ ] At least one scenario tested per risk
- [ ] At least 3 success metrics defined
- [ ] Critical Rules have scenarios
- [ ] Cross-reference with existing agents done

### Spec Gate (before approval)
- [ ] Agent Spec written at correct layer depth
- [ ] Personality examples included
- [ ] Deliverables with format listed
- [ ] All empty fields explicitly marked TBD or N/A
- [ ] Approval items listed
- [ ] Spec presented to user

### Build Gate
- [ ] Spec approved by user
- [ ] Test prompts written for each new capability
- [ ] Happy-path tests pass
- [ ] Boundary tests confirm Critical Rules
- [ ] index.md updated
- [ ] Journal updated
- [ ] Handoff notes written (if not building)
- [ ] Iteration cycle (if needed) documented

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
- [`Aetox-Agents-Team`](https://github.com/aetox-skills/Aetox-Agents-Team)
  — real ZCode agent implementations following this skill's patterns.
- [`Agency-Agents`](https://github.com/msitarzewski/agency-agents)
  — 211 agent archetypes across 16 divisions (external reference).
- `aetox-skills/token-saver` — RTK protocol for token efficiency.
