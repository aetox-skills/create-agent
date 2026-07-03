# Create Agent

**Scope-first AI agent design skill — ถามขอบเขตให้ชัด ก่อนสร้างเอเจน**

<div align="center">

[![GitHub release](https://img.shields.io/github/v/release/aetox-skills/create-agent?style=flat-square)](https://github.com/aetox-skills/create-agent/releases)
[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)
[![GitHub last commit](https://img.shields.io/github/last-commit/aetox-skills/create-agent?style=flat-square)](https://github.com/aetox-skills/create-agent/commits/main)
[![GitHub repo size](https://img.shields.io/github/repo-size/aetox-skills/create-agent?style=flat-square)](https://github.com/aetox-skills/create-agent)
[![GitHub stars](https://img.shields.io/github/stars/aetox-skills/create-agent?style=flat-square)](https://github.com/aetox-skills/create-agent)
[![Static Badge](https://img.shields.io/badge/platform-Claude%20Code_%7C_OpenCode_%7C_Cursor-purple?style=flat-square)]()

</div>

---

## What Makes Create Agent Different?

Most agent builders jump straight into prompt writing, tool config, and code.
Create Agent forces a stop before any of that.

**ถามขอบเขตให้ชัด ก่อนสร้าง** — This is the core.

The skill designs agents across **8 dimensions**:

```
Archetype → Personality → Capabilities → I/O → Ecosystem → Memory → Lifecycle → Risk/Metrics
```

It operates at **4 depth layers**:

```
Layer 0 — Turbo       (infer + confirm + build — สำหรับของง่ายๆ)
Layer 1 — Sketch      (5 quick questions for small changes)
Layer 2 — Standard    (full 8-domain Scope Interview)
Layer 3 — Deep        (adds ecosystem map + dependency trace + learning path)
Layer 4 — Critical    (adds risk model + cost model + safety gates + compliance)
```

With an **Archetype System** (8 archetypes) that adjusts interview weight
per domain — a Developer agent needs different depth than a Scribe.

Plus:
- **Personality framework** — tone, language, verbosity, style, vibe, examples
- **Critical Rules** with concrete scenarios — not just "don't do X"
- **Success Metrics** — every agent must have 3+ measurable KPIs
- **Memory & Learning** — session, persistent, knowledge accumulation
- **Deliverable specs** — concrete output templates per capability
- Checkpoint gates (Intent → Scope → Spec → Build) to prevent skipping

## Use When

- คุณต้องการออกแบบ agent ใหม่ ตั้งแต่ identity → capabilities → ecosystem
- ต้องการปรับปรุง agent ที่มีอยู่ ต้องการ scope ใหม่
- ต้องสร้าง agent spec เพื่อส่งต่อให้ทีมหรือ AI agent อื่นมาสร้างต่อ
- โปรเจคที่ต้องการ agent design review ก่อนลงมือ implement

## Do Not Use When

- แค่แก้ prompt เล็กน้อยใน agent ที่มีอยู่แล้ว
- งาน DevOps หรือ deploy ที่ไม่เกี่ยวกับ agent design
- งานระบบทั่วไป (research, git, config)

## Relationship To Other Aetox Skills

| Skill | ความแตกต่าง |
|-------|------------|
| [`idea-to-architecture-agent`](https://github.com/aetox-skills/idea-to-architecture-agent) | scope-first สำหรับระบบ software ทั่วไป — อันนี้ focus ที่ AI agent โดยเฉพาะ |
| [`docstruct`](https://github.com/aetox-skills/docstruct) | จัดโครงสร้าง docs — อันนี้ design behavior + structure ของ agent |
| [`senior-architect-agent`](https://github.com/aetox-skills/senior-architect-agent) | map ระบบที่มีอยู่แล้ว — อันนี้ design agent ใหม่ตั้งแต่ต้น |

## Quick Use

```bash
# OpenCode (ผ่าน skill system)
skill("create-agent")

# หรือเปิด SKILL.md โดยตรง
```

## Compatibility

- OpenCode — skill system
- Claude Code — วาง SKILL.md ในโปรเจคหรือ `~/.claude/skills/`
- ZCode — วางใน `~/.zcode/skills/`
- Codex — วางใน `.codex/skills/`
- AI agent ทั่วไป — copy content ของ SKILL.md ใส่ system prompt

## Full Workflow

```
┌───────────────────────────────────────────────────┐
│                    START                          │
│  "สร้าง agent X หน่อย" / "ออกแบบ agent ใหม่"    │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ① INTENT GATE                                    │
│ ─────────────────────────────────────────────    │
│ · Restate request                                │
│ · Identify archetype (System / Creative /        │
│   Developer / Scribe / Specialist / Orchestrator │
│   / Researcher / Game)                           │
│ · Check codebase (มี agent อะไรอยู่แล้ว?)         │
│ · Record goal + constraint                       │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ② SELECT LAYER                                   │
│ ─────────────────────────────────────────────    │
│ · Turbo    → infer + confirm + build (ของเล็ก)   │
│ · Sketch   → 5 คำถาม ปรับเล็กน้อย                │
│ · Standard → 8-domain full interview (default)   │
│ · Deep     → Standard + ecosystem map + deps     │
│ · Critical → Deep + risk + cost + safety         │
│ เขียน justification เพราะเลือก layer นี้        │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ③ SCOPE INTERVIEW (8 domains)                    │
│ ─────────────────────────────────────────────    │
│                                                  │
│  [1] IDENTITY         ─── name, archetype,       │
│                          role, persona (tone,    │
│                          language, vibe +        │
│                          ตัวอย่างประโยค)          │
│                                                  │
│  [2] CAPABILITIES     ─── list capabilities      │
│       + BOUNDARIES        ranked, แต่ละตัวมี     │
│                            trigger + output      │
│                            + failure mode        │
│                          Boundaries = Critical   │
│                          Rules (Rule + Scenario  │
│                          + Consequence)          │
│                                                  │
│       │  └─ TOOL MAPPING ⬅── หลัง capabilities    │
│       │     ชัด → แต่ละ capability → tool         │
│       │     category → search codebase → propose │
│       │     categories (ไม่指名 tools)           │
│                                                  │
│  [3] I/O + DELIVERABLES ── input/output format   │
│                            deliverables template  │
│                                                  │
│  [4] ECOSYSTEM         ─── platform, team,       │
│                          dependencies, comms     │
│                                                  │
│  [5] MEMORY & LEARNING ─── session, persistent   │
│                          knowledge accumulation  │
│                                                  │
│  [6] LIFECYCLE         ─── trigger, duration,    │
│                          maintenance, retirement │
│                                                  │
│  [7] RISK + METRICS    ─── failure modes,        │
│                          safety, 3+ KPIs         │
│                          (accuracy, speed,        │
│                           efficiency, quality,    │
│                           business)              │
│                                                  │
│ ── Infer-First: fill spec จากที่มีก่อนถาม ────  │
│    · สังเกต Infer-First เป็น primary technique   │
│    · Decision Tree Walk    (ถามทีละขั้น)          │
│    · Recommend an Answer   (ไม่ถามปลายเปิด)       │
│    · Sharpen Fuzzy Language(คำคลุมเครือ → ชัด)   │
│    · Concrete Scenarios    (ยกเคสสมมุติ)          │
│    · Cross-Reference       (เทียบ agent ที่มี)    │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ④ PROPOSE SPEC                                   │
│ ─────────────────────────────────────────────    │
│ · เขียน Agent Spec 14 sections                    │
│ · Personality examples                           │
│ · Tool categories + proposals                    │
│ · Deliverables + format                          │
│ · Success metrics                                │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ⑤ APPROVAL GATE                                  │
│ ─────────────────────────────────────────────    │
│ · ผู้ใช้ดู spec                                  │
│ · Approve / Revise / Reject                      │
│ · No build until approved                        │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ⑥ BUILD / HANDOFF                                │
│ ─────────────────────────────────────────────    │
│ · ถ้าอนุมัติ → สร้าง agent หรือเขียน handoff notes │
│ · Update index.md + journal                      │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ⑦ AGENT TEST PLAN                                │
│ ─────────────────────────────────────────────    │
│ · เขียน test prompts สำหรับแต่ละ capability        │
│ · Happy path test → ผ่าน                          │
│ · Boundary test (Critical Rules) → ผ่าน            │
│ · Edge case test (Layer 3+)                      │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ⑧ ITERATION LOOP (วนจน stable)                   │
│ ─────────────────────────────────────────────    │
│ · Run test prompts                                │
│ · FAIL → identify gap                            │
│    ├─ Scope gap? → กลับไป redesign spec           │
│    └─ Behavior gap? → fix instruction/rules       │
│ · PASS → deploy                                   │
│ · 3 redesigns fail → escalate                     │
│ · ผู้ใช้บอกพอ → stop                             │
└───────────────────────────────────────────────────┘
```

### One-liner Summary

```
Turbo path:   Infer spec → Confirm → Build
Standard path: Intent Gate → Select Archetype + Layer → Scope Interview
               → Propose Spec → Approval Gate → Build / Handoff → Test Plan → Iteration Loop
```

ตลอดทั้ง process: **Agent Anatomy** เป็นกรอบโครงสร้าง, **Questioning Technique** เป็นเทคนิคการถาม (Infer-First ก่อน), **Checkpoint Gates** คอยกันไม่ให้ข้ามขั้น

## Repository Structure

```
create-agent/
├── README.md        ← ไฟล์นี้
├── SKILL.md         ← ตัวสกิล (รวม Scope Interview + Grill Methodology)
├── INSTALL.md       ← วิธีติดตั้งแต่ละ platform
├── CHANGELOG.md     ← ประวัติการเปลี่ยนแปลง
├── LICENSE          ← MIT
└── templates/
    └── agent-spec.md ← template output spec
```

## Topics

`agent-design` `agent-framework` `ai-agents` `scope-first` `agent-skills`
`claude-code` `opencode` `agent-architecture` `agent-development` `ai-agent-tools`

[Browse all repos in topic →](https://github.com/topics/agent-design)

## License

MIT
