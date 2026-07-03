# Create Agent

**Scope-first AI agent design skill — ถามขอบเขตให้ชัด ก่อนสร้างเอเจน**

---

## What Makes Create Agent Different?

Most agent builders jump straight into prompt writing, tool config, and code.
Create Agent forces a stop before any of that.

**ถามขอบเขตให้ชัด ก่อนสร้าง** — This is the core.

The skill operates at **4 depth layers**, so you never over-engineer a simple
agent or under-scope a complex one:

```
Layer 1 — Sketch      (5 quick questions for small changes)
Layer 2 — Standard    (full 6-domain Scope Interview)
Layer 3 — Deep        (adds ecosystem map + dependency trace)
Layer 4 — Critical    (adds risk model + cost model + safety gates)
```

Every layer produces a validated **Agent Spec** — the difference is completeness.

The skill borrows 7 questioning techniques from `grill-with-docs`:
Decision Tree Walk, One at a Time, Recommend an Answer, Codebase-First,
Sharpen Fuzzy Language, Concrete Scenarios, and Cross-Reference.

Plus checkpoint gates (Intent → Scope → Spec → Build) to prevent skipping.

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

## License

MIT
