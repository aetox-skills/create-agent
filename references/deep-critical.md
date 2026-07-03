# Deep & Critical Layer Reference

> Load this file on demand when the selected layer is Deep or Critical.
> Core SKILL.md covers Quick and Standard layers — this extends with
> Layer 3 and 4 details.

---

## Entry Checklists

### Promote to Deep if any:
- [ ] Agent communicates with other agents or services
- [ ] Agent reads/writes to shared resources (DB, filesystem, APIs)
- [ ] Agent has 2+ external integrations (MCPs, APIs, tools)
- [ ] Agent behavior depends on runtime state or environment
- [ ] Multiple failure modes need explicit handling

### Promote to Critical if any:
- [ ] Agent writes production data
- [ ] Agent handles auth / PII / payment
- [ ] Agent can trigger external side effects (email, deploy, billing)
- [ ] Agent costs significant tokens per call
- [ ] Agent runs on a schedule autonomously
- [ ] Agent deploys to production

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

## Extra Questioning Techniques

These techniques supplement the core T0-T2 (Infer-First, Decision Tree Walk,
Recommend). Use them when the interview reveals ambiguity or risk.

### T3: Sharpen Fuzzy Language

| คำคลุมเครือ | ให้ชัดเป็น |
|-------------|-----------|
| "manage" | read? create? update? delete? |
| "handle" | process? route? store? forward? |
| "data" | what type? what format? what size? |
| "user" | customer? admin? member? |
| "system" | which system? agent? platform? service? |
| "integrate" | API call? webhook? shared DB? event bus? |
| "remember" | session memory? persistent? |

### T4: Concrete Scenarios

When discussing behavior or failure modes — propose a concrete scenario
instead of a theoretical question.

```
"ถ้า agent รับ request ที่ทำไม่ได้ — ควรปฏิเสธ? แนะนำ agent อื่น? หรือส่งต่อไป?"
```

### T5: Cross-Reference

Compare against existing agents before proposing a new design.

```
User: "อยากให้ agent สรุป daily auto"
Cross-ref: "Steward มี daily journal pattern อยู่แล้วใน index.md —
            ใช้ format เดียวกัน หรือออกแบบใหม่?"
```

---

## Token Budget Guidance

Core SKILL.md is ~16KB (~4K tokens). This reference file is ~4KB (~1K tokens).
Total skill package: ~20KB (~5K tokens) when fully loaded.

For most sessions, loading **only** SKILL.md core is sufficient.
Load this file on demand when the selected layer is Deep or Critical.
