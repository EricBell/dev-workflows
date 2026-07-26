# Non-Code Adaptation for `codebase-to-course`

The `codebase-to-course` skill is designed for software projects. This workflow extends it to folders such as `sources/` and other document/topic collections without changing the core course-building mechanics.

## Mapping

| Codebase concept | Non-code/topic equivalent |
| --- | --- |
| App/product | Domain, operational environment, policy system, or body of knowledge |
| User journey | Learner/user decision path, workflow, or process lifecycle |
| Components/modules/services | Documents, roles, systems, rubric dimensions, policies, examples |
| APIs/data flows | Handoffs, decision flows, escalation paths, evaluation workflows |
| Code snippets | Exact source excerpts, tables, criteria, examples, JSON fragments |
| Code ↔ English block | Source/rule/workflow excerpt ↔ plain-English meaning/application |
| Bugs/gotchas | Misinterpretations, edge cases, ambiguity, failure modes, common mistakes |
| Architecture diagram | Concept map, process map, system map, actor/responsibility map |

## Course goal for non-code sources

Teach the learner to use the source material correctly, not merely remember it. Each module should help the learner:

- recognize the important entities and rules
- apply criteria to realistic scenarios
- follow workflows without skipping controls
- identify ambiguity or failure modes
- explain the system/process to another person or AI agent

## Translation block pattern

For non-code sources, use the skill's code translation visual pattern but label it appropriately, for example:

- `Source Says` / `Plain English`
- `Policy Text` / `What You Do With It`
- `Workflow Step` / `Why It Matters`
- `Rubric Criterion` / `How to Apply It`

The left side should be exact or near-exact source material with file/section attribution when feasible. The right side should explain practical meaning.

## Interactive elements

Use the same required interaction types, adapted as follows:

- **Group chat animation:** turn roles/systems/documents into actors discussing a decision.
- **Flow animation:** animate process steps, decision gates, escalation paths, or evaluation flow.
- **Quiz:** scenario-based application questions, not recall questions.
- **Glossary tooltip:** domain terms, system names, acronyms, workflow labels.

## Source fidelity rules

- Do not invent rules not present in the source.
- If a source is ambiguous, teach the ambiguity explicitly.
- Preserve names of systems, roles, workflows, and rubric dimensions exactly.
- If multiple sources conflict, include the conflict as a teaching point or limitation.
