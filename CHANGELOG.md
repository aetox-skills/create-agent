# Changelog

## v0.5.1 (2026-07-03)

- **Default layer changed to Quick** — every session starts at Quick, escalate
  to Standard only when complexity signals appear
- **Interview style: present draft → user marks up** — instead of "one question
  at a time" across 8 domains, AI infers a draft spec and asks only gaps
- **Iron Rule #5 renamed** — "One question at a time" → "Minimize round-trips"
- **Iron Rule #7 reworded** — scope to spec approval only (interview responses
  don't need formal documentation)
- **Archetype catalog** column renamed: "Default Layer" → "Suggested Layer" with
  Quick as starting point for most archetypes
- **Workflow diagram** updated: Quick labeled DEFAULT, Standard path redesigned
  (draft → checklist gaps → propose → approve)
- All changes from Opus 4.6 round-2 feedback re: reducing ceremony tax

## v0.5.0 (2026-07-03)

**Major refactor based on Opus 4.6 feedback.** Key changes:

- **4 layers (real)** — Turbo + Sketch merged into Quick. SKILL.md now says "4"
  and means it. No more inconsistency.
- **File split** — `SKILL.md` (core, ~500 lines, Quick + Standard) +
  `references/deep-critical.md` (Deep/Critical layers, on-demand).
  Reduces token waste when loading the skill.
- **Archetype skip list** — weight matrix (High/Medium/Low) replaced with
  actionable skip list: "archetype X → skip domains Y, Z. Focus on A, B."
- **Persona simplified** — 5 dimensions → 2 (Tone + Example phrase).
- **Tool mapping** — propose specific tool if known, category if not.
- **Checkpoint gates 4→2** — Intent Gate + Spec Gate only. Scope/Build gates
  removed (redundant with interview flow and build process).
- **Iron Rule #7 fix** — Quick-layer confirmation counts as explicit agreement.
- **Inline template removed** — `templates/agent-spec.md` is single source
  of truth. SKILL.md points to it.
- **Success metrics optional** — suggested 1+, not mandatory 3.
- **Greenfield path** — if no codebase exists, skip scan, go to interview.
- **Diff spec** — when modifying existing agent, output only changed fields.
- **Standard walkthrough example** — PR reviewer agent walkthrough from
  Intent Gate through Approval.
- **Questioning technique** — cut to T0 (Infer-First) + T1 (Decision Tree)
  + T2 (Recommend). T3-T5 moved to references/deep-critical.md.

## v0.4.1 (2026-07-03)

- **Turbo Mode (Layer 0)** — for dead-simple agents: infer → confirm → build
- **Infer-First rule** — fill spec from context + codebase before asking anything
- **Grill Methodology → Questioning Technique** — retitled and simplified; Infer-First (T0) as primary
- **Workflow diagram updated** — Turbo path skips approval gate; Standard path retains it
- **Layer selection table** — added Turbo column with entry signals
- **SKILL.md template** — minor refinements (frontmatter trim, dimension order, rule#2 clarity)
- All changes respond to Prof./อาจารย์ feedback: reduce friction, infer before asking, turbo for small tasks

## v0.4.0 (2026-07-03)

- **Agent Anatomy (Progressive Disclosure)** — 3-level structure: Identity → Instruction → Resource
- **Tool Categories** — directional guidance (ไม่指名 specific tools): knowledge retrieval, code execution, communication, system ops, data processing, visual/creative, monitoring
- **Tool Mapping process** — หลัง capabilities + boundaries ชัด → map แต่ละ capability → tool categories → search codebase → propose
- **Agent Test Plan** — test prompts per capability with format, example, edge cases
- **Agent Iteration Loop** — Build → Test → Review → Redesign → Repeat
- Iteration documentation standard (what changed, before/after, test results, known issues)
- Build Gate expanded: test prompts, happy-path, boundary tests, iteration docs
- All patterns adapted from `skill-creator` (test/evals, iteration, progressive disclosure, lean principle)

## v0.3.0 (2026-07-03)

- **Agent Archetype System** — 8 archetypes (System, Creative, Developer, Scribe, Specialist, Orchestrator, Researcher, Game) with domain weight table
- **Personality Framework** — tone, language, verbosity, style, vibe, with example phrases (inspired by Agency-Agents 211-agent patterns)
- **Critical Rules with Scenarios** — Rule + Scenario + Consequence format for boundaries (from Agency-Agents safety pattern)
- **Success Metrics** — 5 metric types (Accuracy, Speed, Efficiency, Quality, Business) with target/measure/alert per metric
- **Memory & Learning domain** — session memory, persistent memory, knowledge accumulation, forgetting rules
- **Deliverables section** — concrete output templates per capability/archetype
- **Expanded Agent Spec template** — 14 sections matching the new 8-domain interview
- Checkpoint Gates updated: Archetype, Persona, Metrics, Critical Rules added to Scope/Spec gates
- Grill Methodology expanded: +Sharpen for "personality" and "remember" terms
- Related section: linked Aetox-Agents-Team + Agency-Agents as reference

## v0.2.0 (2026-07-03)

- **Multi-layer design system** — 4 depth layers: Sketch, Standard, Deep, Critical
- Layer selection table with entry criteria for each layer
- Escalation rule: promote layer when complexity signals emerge
- **Checkpoint Gates** — Intent, Scope, Spec, Build — must pass before moving on
- Expanded Workflow: Intent Gate → Select Layer → Scope Interview → Propose Spec → Approval Gate → Build/Handoff
- Agent Spec template (templates/agent-spec.md) with layer-conditional sections
- Risk & Safety section (Layer 3+): failure register, cost model, safety gates
- Ecosystem Map + Dependency Trace + Interaction Design (Layer 3+)
- Revised Grill Methodology integration per layer

## v0.1.0 (2026-07-03)

- Initial release: scope-first agent design skill
- Scope Interview Checklist (6 หมวด: Identity, Capabilities, I/O, Ecosystem, Lifecycle, Risk)
- Grill Methodology (7 เทคนิค: Decision Tree Walk, One at a Time, Recommend an Answer, Codebase-First, Sharpen Fuzzy Language, Concrete Scenarios, Cross-Reference)
- 3-Phase Workflow: Scope Interview → Propose Spec → Approval
- Agent Spec Template
