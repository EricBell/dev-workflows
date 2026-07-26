# Problem Lens for Course Building

Use this reference when the user wants a course that is about finding problems in a repo, folder, or source set.

## Core idea

The course is not a tour of the system. It is a guided audit of what is wrong, risky, inefficient, stale, missing, or misleading.

## What counts as a problem

Prioritize concrete, source-grounded issues such as:

- correctness bugs
- authorization or validation gaps
- performance inefficiencies
- dead code, unused paths, or stale branches
- documentation drift
- skipped, missing, or weak tests
- objective mismatch: the implementation does not match the stated goal
- maintainability hazards and repeated patterns that increase blast radius

## How to shape the course

Each module should answer one question:

- What is the problem?
- Where is it in the source?
- Why does it matter?
- How would you explain it to a teammate?

## Translation block pattern

Use the normal course translation block, but make the right side about interpretation and impact:

- `Source Says` / `Plain English`
- `Code` / `Why this is a problem`
- `Workflow Step` / `What could go wrong`
- `Policy Text` / `How to apply it safely`

## Tone

Be specific, evidence-based, and practical. Avoid vague “code smell” language unless the source itself is genuinely ambiguous.

## Naming

If the base topic is `topic-name`, the course slug should be `topic-name-problems-course`.
