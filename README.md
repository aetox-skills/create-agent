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
Layer 1 — Quick      (infer + confirm + build — สำหรับของง่ายๆ)
Layer 2 — Standard   (full 8-domain Scope Interview — default)
Layer 3 — Deep       (Standard + ecosystem map + dependency trace)
Layer 4 — Critical   (Deep + risk model + cost model + safety gates)
```

With an **Archetype System** (8 archetypes) with **skip lists** — telling
each archetype which interview domains to skip, not relative weights.

Plus:
- **Iron Rules** — scope-first, infer-before-ask, minimum-sufficient-depth
- **Infer-First questioning** — fill spec from context before asking
- **Quick path** — for trivial agents: infer → confirm → build, no gate
- **Diff spec** — when modifying, output only changed fields
- **Checkpoint gates** (Intent → Spec) — light enough for daily use

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
│ · Check codebase (skip if greenfield)            │
│ · Record goal + constraint                       │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ② SELECT LAYER                                   │
│ ─────────────────────────────────────────────    │
│ · Quick    → infer + confirm + build (ของเล็ก)   │
│ · Standard → 8-domain full interview (default)   │
│ · Deep     → Standard + ecosystem map + deps     │
│ · Critical → Deep + risk + cost + safety         │
│ เขียน justification เพราะเลือก layer นี้        │
└───────────────────────────────────────────────────┘
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
  ┌─────────────────┐   ┌─────────────────────────────┐
  │ QUICK PATH      │   │ STANDARD / DEEP / CRITICAL  │
  │                 │   │                             │
  │ 3. INFER +      │   │ 3. SCOPE INTERVIEW          │
  │    CONFIRM      │   │   · Apply archetype skip    │
  │   Infer spec →  │   │     list                    │
  │   เสนอ → user   │   │   · 8 domains (skip per    │
  │   confirm →     │   │     archetype)              │
  │   BUILD         │   │   · Deep/Critical = extra   │
  │                 │   │     steps per references/   │
  └─────────────────┘   │     deep-critical.md        │
          │             │                             │
          │             │ 4. PROPOSE SPEC             │
          │             │   · templates/agent-spec.md │
          │             └─────────────┬───────────────┘
          │                           │
          │             ┌─────────────▼───────────────┐
          │             │ 5. APPROVAL GATE            │
          │             │   · Approve / Revise /      │
          │             │     Reject                  │
          │             │   · No build until approved │
          │             └─────────────┬───────────────┘
          │                           │
          └───────────┬───────────────┘
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
│ · Happy path test, Boundary test, Edge case       │
└───────────────────────────────────────────────────┘
                      │
                      ▼
┌───────────────────────────────────────────────────┐
│ ⑧ ITERATION LOOP (วนจน stable)                   │
│ ─────────────────────────────────────────────    │
│ · Run test prompts                                │
│ · FAIL → identify gap → redesign / fix            │
│ · PASS → deploy                                   │
│ · 3 redesigns fail → escalate                     │
│ · ผู้ใช้บอกพอ → stop                             │
└───────────────────────────────────────────────────┘
```

### One-liner Summary

```
Quick path:    Infer spec → Confirm → Build
Standard path: Intent Gate → Select Layer → Scope Interview
               → Propose Spec → Approval Gate → Build / Handoff → Test Plan → Iteration Loop
```

ตลอดทั้ง process: **Iron Rules** (scope-first, infer-first), **Questioning Technique** (Infer-First + Recommend), **Checkpoint Gates** (Intent → Spec)

## Repository Structure

```
create-agent/
├── README.md              ← ไฟล์นี้
├── SKILL.md               ← ตัวสกิล (core ~600 lines — Quick + Standard)
├── references/
│   └── deep-critical.md   ← Deep & Critical layer details (เปิดเมื่อจำเป็น)
├── templates/
│   └── agent-spec.md      ← output template (single source of truth)
├── INSTALL.md             ← วิธีติดตั้งแต่ละ platform
├── CHANGELOG.md           ← ประวัติการเปลี่ยนแปลง
└── LICENSE                ← MIT
```

**Token budget:** Core SKILL.md ~16KB (~4K tokens). References ~4KB (~1K tokens).
โหลด references/ เฉพาะตอนใช้ Deep/Critical layer — ลด token waste สำหรับ Quick/Standard.

## Topics

`agent-design` `agent-framework` `ai-agents` `scope-first` `agent-skills`
`claude-code` `opencode` `agent-architecture` `agent-development` `ai-agent-tools`

[Browse all repos in topic →](https://github.com/topics/agent-design)

## License

MIT
