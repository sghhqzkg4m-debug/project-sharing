---
name: prd-to-knowledge
description: Convert long, messy, or semi-structured product requirement documents into modular Markdown knowledge files for UX/UI design, frontend implementation, and product alignment, including screenshots and generated diagrams when they make the output clearer. Use when Codex is asked to analyze PRDs, extract requirements, decompose features, turn product specs into a knowledge base, summarize user journeys, define information architecture, identify edge cases, create per-feature design/development context, preserve PRD screenshots, or generate flowcharts, relationship diagrams, state diagrams, or visual explanations from requirement docs.
---

# PRD to Knowledge

## Overview

Transform raw PRDs into concise, high-signal knowledge artifacts. Prioritize context that helps UX/UI designers and frontend engineers make good decisions: goals, user journeys, information architecture, interaction behavior, states, constraints, visual evidence, diagrams, and open questions.

## Workflow

1. **Ingest and deconstruct**: Read the source PRD deeply. Identify independent feature modules, user-facing objects, workflows, panels, pages, permissions, and stateful entities.
2. **Extract visual evidence**: If the source file contains screenshots, diagrams, architecture images, tables, or annotated flows, extract or preserve the useful images under `0_Knowledge/assets/` or a user-specified assets folder. Name images with the related module slug, such as `model-square-list-screenshot.png`.
3. **Filter noise**: Remove meeting notes, project scheduling, vague visual preferences, stakeholder opinions without product logic, and duplicated wording. Preserve business rules, user intent, constraints, screenshots that clarify behavior, and unresolved ambiguity.
4. **Split into files**: Create one Markdown file per feature module under `0_Knowledge/` unless the user specifies another target directory. Do not collapse all modules into one long document.
5. **Name files consistently**: Use lowercase English slugs with hyphens, such as `0_Knowledge/entity-list-restructure.md`. If the source feature name is Chinese, translate the filename but keep the original feature title inside the document.
6. **Choose text or visuals per topic**: For each module, decide whether a screenshot, generated diagram, table, or text explanation is clearest. Use diagrams for relationships, flow, state transitions, and architecture; use screenshots for concrete UI evidence; use text for simple rules or ambiguous content.
7. **Structure each module**: Follow the single-file output format in `references/output-format.md`.
8. **Surface ambiguity**: Add explicit open questions when the PRD lacks users, triggers, permissions, data rules, empty states, error states, recovery paths, or when a screenshot is unclear.
9. **Keep it implementation-useful**: Write for downstream design and frontend work. Extract actual behavior and constraints instead of producing generic product summaries.

## Module Detection

Treat each of these as a likely separate knowledge file:

- A distinct page, panel, flow, or feature area.
- A reusable entity or list that has its own filters, states, permissions, or actions.
- A workflow with a clear beginning, happy path, and alternative paths.
- A configuration surface, parameter panel, or rule editor.
- A meaningful frontend component family, such as table management, batch operation, import/export, search/filter, review/approval, or notification.

Merge modules only when they cannot be understood or implemented independently.

## Output Rules

- Create Markdown files, not a single giant answer, when the user provides a substantial PRD or asks for a knowledge base.
- Start each file with `Feature: [module name]`.
- Use clear Chinese headings when the input is Chinese; preserve essential English domain terms in parentheses when useful.
- Prefer bullets and numbered flows over prose blocks.
- Include concrete edge cases, alternative paths, empty/loading/error states, permission limits, and data constraints.
- Add a `视觉辅助 (Visual Aids)` section when an image or diagram makes the requirement easier to understand.
- Prefer Mermaid diagrams in Markdown for flows, system relationships, state machines, sequence interactions, and information architecture. Use screenshots only when the original image contains UI details, annotations, or visual evidence that cannot be faithfully reconstructed in text.
- Reference extracted screenshots with relative Markdown image paths, such as `![模型广场列表截图](assets/model-square-list-screenshot.png)`.
- Add a one-line takeaway before or after each visual so the reader knows what to look for.
- If a visual would add noise, explicitly keep the explanation textual and do not force a diagram.
- Avoid inventing business rules. Mark inferences as `推断` and unknowns as `待确认`.
- Do not include project timeline, staffing, meeting logistics, or generic UI taste unless it affects product behavior.

## Visual Decision Rules

- Use a **screenshot** when the PRD image shows actual UI layout, labels, controls, error states, annotations, or design intent.
- Use a **flowchart** when the topic is a user journey, happy path, exception path, approval path, import/export path, deployment path, or cross-system handoff.
- Use a **relationship graph** when the topic involves products, modules, systems, roles, data entities, permissions, or ownership boundaries.
- Use a **sequence diagram** when timing or back-and-forth interaction matters, such as frontend -> gateway -> cloud -> model repository.
- Use a **state diagram** when an entity has meaningful states, such as model upload status,上架/下架状态, deployment status, task status, or approval status.
- Use a **table** when comparing options, rules, permissions, constraints, metrics, or feature differences.
- Use **text only** when the content is short, linear, uncertain, or when a diagram would repeat the same information with more visual clutter.

## Mermaid Guidance

- Keep diagrams small enough to scan. Prefer 5-12 nodes; split larger diagrams.
- Use Chinese node labels when the output language is Chinese.
- Quote labels that contain punctuation, parentheses, slashes, or long phrases.
- Do not use Mermaid to invent missing steps. Mark inferred edges with `推断` in nearby text.
- Use these Mermaid types:
  - `flowchart TD` for user flows and process flows.
  - `flowchart LR` for module/entity relationships.
  - `sequenceDiagram` for cross-system interactions.
  - `stateDiagram-v2` for lifecycle/status changes.

## References

- Read `references/output-format.md` before writing final knowledge files or when the user asks for a complete reusable output structure.
