# Install — Create Agent

## Prerequisites

None. This is a knowledge skill — no tools, no dependencies.

## OpenCode

```bash
skill("create-agent")
```

หรือวาง `SKILL.md` ไว้ที่ `~/.config/opencode/skills/create-agent/SKILL.md`

## Claude Code

วาง `SKILL.md` ไว้ที่ `~/.claude/skills/create-agent/SKILL.md`
หรือวางในโปรเจคที่ `.claude/skills/create-agent/`

## ZCode

วาง `SKILL.md` ไว้ที่ `~/.zcode/skills/create-agent/SKILL.md`

## Codex

วาง `SKILL.md` ไว้ที่ `.codex/skills/create-agent/SKILL.md`

## AI Tool อื่นๆ

ก็อปปี้ content จาก `SKILL.md` ใส่ system prompt หรือ instruction ของ agent

## Verify

```bash
# เปิดไฟล์ SKILL.md แล้วอ่าน — ถ้าขึ้นว่า name: create-agent แสดงว่าพร้อมใช้
head -3 SKILL.md
```
