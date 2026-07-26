# Source Ingestion Output Format

Use this structure for source ingestion artifacts.

```markdown
# Source Ingestion: <title/topic>

## Ingested Files
- `sources/<file>` — <status/coverage note>

## Executive Digest
<Short summary of what the source material is for and why it matters.>

## Key Facts
- <Fact grounded in the source>

## Important Entities / Systems / Roles
| Name | Type | What it does / why it matters |
| --- | --- | --- |

## Processes / Workflows Described
### <Process name>
- Trigger:
- Actors:
- Steps:
- Outputs:
- Controls / constraints:

## Rules, Policies, or Evaluation Criteria
- <Rule or criterion>

## Terminology
| Term | Meaning |
| --- | --- |

## Cross-References
- <Relationships to other source files, if known>

## Ambiguities / Open Questions
- <What is unclear, missing, or potentially conflicting>

## Downstream Use Notes
- <How future workflows should use this source>
```

Guidelines:

- Be faithful to the source. Do not invent unstated rules.
- Prefer compact, structured notes over prose when possible.
- Preserve exact names of systems, workflows, roles, and criteria.
- Use quotes sparingly, only for important wording.
- If source material conflicts with another ingested file, flag it rather than resolving it silently.
