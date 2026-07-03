# Changelog

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
