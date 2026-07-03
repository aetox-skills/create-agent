---
name: create-agent
description: >-
  ออกแบบและสร้าง AI agent โดยยึดหลัก "ถามขอบเขตให้ชัด ก่อนสร้าง"
  ใช้เมื่อพี่ Mike บอกให้ออกแบบ agent ใหม่, ปรับปรุง agent ที่มีอยู่,
  หรือพูดถึง concept agent ไหนก็ตามที่ต้องการให้สร้าง/ออกแบบ
  ห้ามตอบรับแล้วลงมือสร้างทันที — ต้องผ่าน Scope Interview ก่อนเสมอ
---

# Create Agent Skill

## แก่นของสกิลนี้

**ถามขอบเขตให้ชัด ก่อนสร้างเอเจน**

ห้ามเดา ห้ามข้ามขั้น ห้ามคิดแทนพี่ Mike ถ้าข้อมูลไม่พอ — ถามก่อน
ถ้าข้อมูลพอแต่พี่ Mike บอกว่า "เดี๋ยวค่อยทำ" — หยุดก่อน

---

## กฎเหล็ก (Must Follow)

1. **Scope Interview ก่อนเสมอ** — ยังไม่ต้องเขียนโค้ด, ยังไม่ต้องออกแบบ prompt,
   ยังไม่ต้องเสนอ tech stack จนกว่าขอบเขตจะชัด
2. **ถามให้ละเอียด** — อย่าถามแค่ "อยากได้ agent ทำอะไร"
   ถามย่อยลงไปจนมั่นใจว่าขอบเขตไม่มีช่องโหว่
3. **จับให้ได้ว่าแก่นของ agent คืออะไร** — identity, บทบาท,
   สิ่งที่ทำได้/ทำไม่ได้, output ที่คาดหวัง
4. **จับให้ได้ว่า agent ตัวนี้อยู่ใน ecosystem ไหน** — อยู่คนเดียว? เป็นทีม?
   คุยกับ agent อื่น? ต้องใช้ MCP/Tools อะไร?
5. **เสนอโครงสร้างก่อนสร้าง** — เมื่อขอบเขตชัดแล้ว ให้เสนอโครงสร้าง agent (spec)
   ให้พี่ Mike ดูอนุมัติก่อนลงมือ
6. **ถ้าพี่ Mike บอกพอแล้ว / แค่นี้ก่อน** — หยุด อย่าเถียง อย่าเสนอเพิ่ม

---

## เทคนิคการถาม (Grill Methodology)

เทคนิคเหล่านี้ยืมมาจาก `grill-with-docs` ปรับมาใช้กับ Scope Interview
เป้าหมายคือปิดช่องโหว่ของขอบเขตให้มากที่สุด ก่อนลงมือสร้าง

### 1. Decision Tree Walk

**ถามทีละคำถาม เรียงตาม dependency** — อย่าถามรวดเดียว 5 คำถาม
แต่ละคำถามควรต่อเนื่องจากคำตอบก่อนหน้า

```
ตัวอย่าง:
  ตอบว่า "agent นี้ต้องเชื่อมต่อ API" ✅ →
    → แล้วถามต่อว่า "API ไหน? มี docs หรือยัง?"
    → แล้วถามต่อว่า "authentication แบบไหน?"
    → แล้วถามต่อว่า "rate limit มีไหม?"
```

**ห้ามกระโดดข้ามชั้น** — เช่น ถามเรื่อง deployment ทั้งที่ identity ยังไม่ชัด

### 2. One Question at a Time

ถามทีละ 1 คำถาม รอพี่ Mike ตอบก่อน **แล้วค่อยถามต่อ**

- ข้อดี: พี่ Mike ไม่ต้องจำหลายคำถาม, ได้คำตอบที่มีคุณภาพกว่า
- ข้อยกเว้น: คำถามที่ตอบได้จากการเช็ค codebase → ไม่ต้องถาม (ดูข้อ 4)

### 3. Recommend an Answer (ไม่ถามปลายเปิด)

ทุกคำถามที่ถาม ต้องมี **ข้อเสนอแนะ** ว่าเราคิดว่าควรเป็นยังไง
เพื่อให้พี่ Mike มีอะไรให้ react  ไม่ใช่ถามแล้วพี่ Mike ต้องคิดเองทั้งหมด

```
❌ "อยากให้ agent มี personality แบบไหน?"
✅ "ผมว่า tone ทางการ + ใช้ภาษาไทยน่าจะเหมาะกับงานนี้
    หรือพี่อยากได้แนวอื่น? เช่น casual หรือ technical English?"
```

### 4. Codebase-First (อย่าถามสิ่งที่เช็คได้)

ถ้าคำตอบของคำถามใด **หาได้จาก codebase, config, หรือ docs ที่มีอยู่**
ให้ไปค้นมาก่อน — อย่าถามพี่ Mike

```
ตัวอย่าง:
  - "agent นี้ต้องอยู่ใน ecosystem ไหน?"
    → เช็ค opencode.jsonc, index.md, AGENTS.md ก่อนถาม
  - "มี agent อะไรอยู่แล้วบ้าง?"
    → เช็ค index.md รายชื่อ agent และ skill-library ก่อน
```

**สิ่งที่เช็คได้เสมอ:**
- `index.md` — เอเจนที่มีอยู่, โครงสร้างระบบ
- `CONTEXT.md` — env, paths, tools
- `opencode.jsonc` — config, MCPs, plugins
- `AGENTS.md` — กฎการทำงานของเอเจน
- `PROFILE.md` — ข้อมูลพี่ Mike
- `skill-library/` — skills ที่มีอยู่

### 5. Sharpen Fuzzy Language

เมื่อพี่ Mike ใช้คำที่คลุมเครือ หรือมีความหมายได้หลายแบบ —
**จับแล้วถามให้ชัด** อย่าเดาเอาเอง

```
พี่ Mike: "อยากได้ agent ที่จัดการ email"
   คำว่า "จัดการ email" กว้างมาก →
   ถาม: "พี่หมายถึงอ่านเฉยๆ? ตอบกลับ? สรุป?
         filter spam? หรือ automate workflow?"
```

**คำที่พบบ่อยว่าคลุมเครือ:**
- "manage", "handle", "process" → จัดการยังไง?
- "data", "content", "info" → data อะไร? รูปแบบไหน?
- "user", "customer", "client" → ใคร? ใช้ยังไง?
- "system", "platform", "service" → หมายถึงตัวไหน?
- "integrate", "connect", "link" → ต่อยังไง? direction ไหน?

### 6. Concrete Scenarios

เมื่อต้องตัดสินใจเกี่ยวกับพฤติกรรมของ agent —
**ยก scenario เป็นรูปธรรม** เพื่อให้พี่ Mike เห็นภาพและตัดสินใจได้ตรง

```
ถาม: "สมมุติ agent รับคำสั่งที่ทำไม่ได้ เช่น 'รันโค้ด Python'
     แต่ agent ตัวนี้ไม่มีสิทธิ์รันโค้ด — ควรตอบยังไง?"
     a) บอกว่า "ทำไม่ได้" เฉยๆ
     b) แนะนำ agent อื่นที่ทำได้
     c) ขอสิทธิ์ก่อนค่อยทำ
```

### 7. Cross-Reference

เมื่อพี่ Mike บอกว่า agent ควรทำอะไรหรือมีคุณสมบัติอะไร —
**เช็คกับของที่มีอยู่จริง** ว่ามีอะไรที่ขัดแย้งหรือซ้ำซ้อนไหม

```
พี่ Mike: "อยากได้ agent สรุป email"
   เช็ค: ใน index.md มี agent ไหนที่ทำ email อยู่แล้ว?
         ถ้ามี → ถามว่า "อันใหม่ หรือเอาอันเดิมมาปรับ?"
         ถ้าไม่มี → ถามต่อเรื่องขอบเขต
```

---

## Scope Interview Checklist

ทุกครั้งก่อนออกแบบ agent ต้องตอบคำถามเหล่านี้ให้ครบ:
ใช้เทคนิค Grill Methodology ข้างต้นในการถาม

### 1. Identity & Purpose
- [ ] Agent นี้ชื่ออะไร? (ถ้ายังไม่มี — เสนอชื่อให้)
- [ ] บทบาทคืออะไร? ทำหน้าที่อะไรในระบบ?
- [ ] ต้องรู้บริบทอะไรก่อนทำงาน? (โปรเจค? repo? env?)
- [ ] personality / tone แบบไหน? (ทางการ? casual? technical?)
- [ ] ใช้ภาษาไทย? อังกฤษ? หรือผสม?

### 2. Capabilities & Boundaries
- [ ] ทำอะไรได้บ้าง? (list ความสามารถหลัก — ลอง sort priority)
- [ ] ทำอะไรไม่ได้ / ไม่ควรทำ? (เทคนิค 6: ยก scenario ขอบเขตให้เห็นภาพ)
- [ ] ใช้ tools/MCPs อะไร?
- [ ] ต้องอ่าน skill/knowledge อะไรก่อนทำงาน?
- [ ] มี agent ตัวอื่นทำอะไรคล้ายๆ นี้อยู่แล้วไหม? (เทคนิค 7: cross-ref)

### 3. Input / Output
- [ ] รับ input แบบไหน? (text? file? command? voice? webhook?)
- [ ] ส่ง output แบบไหน? (file? message? API? action? email?)
- [ ] มี format ที่ต้อง遵守ไหม?
- [ ] synchronous หรือ asynchronous?

### 4. Ecosystem
- [ ] อยู่คนเดียวหรือเป็นทีม?
- [ ] คุยกับ agent ไหนบ้าง?
- [ ] dependencies อะไร? (API keys? services? env?)
- [ ] อยู่ตรงไหนของระบบ? (config? plugin? subagent?)
- [ ] เช็ค index.md, opencode.jsonc ก่อนถาม (เทคนิค 4: codebase-first)

### 5. Lifecycle
- [ ] ใช้ตอนไหน? (ทุกครั้ง? ตามคำสั่ง? trigger? schedule?)
- [ ] ต้อง auto-load หรือ on-demand?
- [ ] maintenance มีใครดูแล? ถ้าไม่ใช้แล้วต้องเอาออก?
- [ ] log / memory / journal ต้องมีไหม?

### 6. Risk & Constraints
- [ ] อะไรคือ failure mode? (เทคนิค 6: ยก scenario failure)
- [ ] มีข้อมูล sensitive ไหม? (API keys? user data?)
- [ ] cost / token budget?
- [ ] safety guardrails?
- [ ] ต้องมี approval step ก่อน action สำคัญไหม?

---

## Workflow

```
พี่ Mike: "สร้าง agent X ให้หน่อย"
         │
         ▼
   ┌────────────────────────────────────┐
   │        Phase 1: Scope Interview    │
   │  ────────────────────────────────  │
   │  · เดิน decision tree ทีละคำถาม    │
   │  · เช็ค codebase ก่อนถามเสมอ       │
   │  · ทุกคำถามมี recommendation       │
   │  · ลับคำคลุมเครือให้คม             │
   │  · ยก scenario ให้เห็นภาพ         │
   │  · cross-ref กับของที่มีอยู่       │
   └────────────────────────────────────┘
         │
         ├── ข้อมูลไม่พอ → ถามต่อ (ทีละคำถาม)
         ├── ข้อมูลพอ → สรุปให้พี่ Mike ตรวจ
         └── พี่ Mike บอกพอ → หยุด
         │
         ▼
   ┌────────────────────────────────────┐
   │        Phase 2: Propose Spec      │
   │  ────────────────────────────────  │
   │  identity + capabilities          │
   │  tools / MCPs                     │
   │  input / output                   │
   │  placement in ecosystem           │
   │  lifecycle + risks                │
   └────────────────────────────────────┘
         │
         ▼
   ┌────────────────────────────────────┐
   │        Phase 3: Approval          │
   │  ────────────────────────────────  │
   │  พี่ Mike ดู spec → อนุมัติ?       │
   │  Yes → ลงมือสร้าง                  │
   │  No → แก้ spec ตาม feedback        │
   └────────────────────────────────────┘
```

---

## Output Template (Agent Spec)

เมื่อ Scope Interview จบและพี่ Mike อนุมัติ ให้สร้าง agent spec ใน format นี้:

```markdown
# Agent Spec: [ชื่อ]

## Identity
- บทบาท:
- personality:
- รู้บริบท:

## Capabilities
- [ ] ความสามารถหลัก 1
- [ ] ความสามารถหลัก 2
- Boundaries: (อะไรที่ไม่ควรทำ)

## Tools & MCPs
- [tool/mcp ที่ต้องใช้]
- [API keys หรือ env ที่ต้องมี]

## Input / Output
- Input format:
- Output format:

## Ecosystem
- อยู่ในทีม: (steward? designer? เดี่ยว?)
- คุยกับ: (agent ไหนบ้าง)
- อยู่ที่: (path ในระบบ)

## Lifecycle
- ใช้เมื่อ:
- Auto-load?:
- Maintenance:

## Risks
- failure modes:
- cost notes:
```

---

## ข้อควรจำ

- ถ้าพี่ Mike พูดคลุมเครือ — ถามให้ชัดก่อน (เทคนิค sharpen)
- ก่อนถาม → เช็ค codebase ก่อน (เทคนิค codebase-first)
- ทุกคำถาม → มี recommendation เสมอ
- ถ้าพี่ Mike พูดว่า "ช่างมันก่อน" — หยุด
- อย่าเติมของที่พี่ Mike ไม่ได้ขอ
- อย่าคิดแทนพี่ Mike ว่า "น่าจะต้องการอะไรเพิ่ม"
- ถ้าสร้างเสร็จ → อัพเดท index.md และ journal
