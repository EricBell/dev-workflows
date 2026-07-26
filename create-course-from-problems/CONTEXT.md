# Workflow: Create Course from Problems / Folder

## Purpose
Create an interactive class/course from a requested topic, file, folder, or codebase by auditing it for problems, not by teaching its happy-path behavior. Use the Claude skill at:

```text
~/.claude/skills/codebase-to-course
```

This workflow preserves the skill’s codebase-oriented course builder, but changes the lens: the course should help the learner inspect a repo or source collection for bugs, inefficiencies, dead code, drift, gaps, and objective mismatches.

## Trigger
Use this workflow when the user asks to create, spec, design, or build a class/course/tutorial/walkthrough from:

- a codebase or project folder, with the goal of finding problems
- a specific file in `sources/`, with the goal of finding problems
- multiple files in `sources/`, with the goal of finding problems
- a nominal folder such as `sources/`
- a topic represented by source files or previous ingestion outputs

Typical wording includes: audit, investigate problems, find issues, surface inefficiencies, identify dead code, check for drift, or compare what exists to what should exist.

## Inputs

| Layer | Path | Required | Notes |
| --- | --- | --- | --- |
| Layer 3 reference | `~/.claude/skills/codebase-to-course/SKILL.md` | Yes | Primary skill instructions. Follow unless this workflow explicitly adapts the task. |
| Layer 3 reference | `~/.claude/skills/codebase-to-course/references/` | Yes | Copy/build/reference assets as required by the skill. Read reference files only when the skill says they are relevant. |
| Layer 3 reference | `workflows/create-course-from-problems/references/problem-lens.md` | Yes | Defines how to turn findings into a problem-focused course. |
| Layer 4 working | requested file/folder/topic | Yes | May be a codebase, `sources/`, a specific source file, or an ingestion artifact. |
| Layer 4 working | `workflows/source-ingestion/output/` | Optional | Use if the topic has already been digested there. |

## Process

### 1. Resolve the course source

Identify whether the requested input is:

- **Codebase mode** — a software repo/project with code, entry points, components, APIs, etc.
- **Nominal folder mode** — a folder of documents or mixed source material, such as `sources/`.
- **File/topic mode** — one or more files or a topic grounded in files.

### 2. Read the skill

Read `~/.claude/skills/codebase-to-course/SKILL.md` and follow its phases:

1. Analyze source material
2. Design curriculum
3. Build the course directory
4. Assemble with `build.sh`
5. Review/open/report

Do not regenerate the skill’s prebuilt CSS/JS/build assets. Copy them from the skill references as instructed.

### 3. Analyze source material as problem surface

#### Codebase mode
Do not treat the repo as a tutorial subject to explain end-to-end. Treat it as an artifact to inspect for problems.

Look for:

- correctness bugs and broken flows
- authorization, validation, or safety gaps
- performance issues and avoidable inefficiencies
- dead code, unused paths, stale flags, and commented-out logic
- documentation drift and misleading instructions
- skipped tests, weak coverage, or brittle CI assumptions
- mismatches between the stated objective and the implementation
- confusing interfaces, repeated code, or maintenance hazards

Ground every problem in the actual source. Prefer concrete files, lines, and excerpts over general smell language.

#### Nominal folder or file/topic mode
Apply the non-code adaptation, but keep the problem lens:

- Treat documents, systems, policies, rubrics, workflows, roles, and examples as the “actors.”
- Treat broken procedures, omissions, contradictions, weak controls, and ambiguity as the “problems.”
- Treat criteria, constraints, and definitions as the “rules of the system.”
- Use source excerpts instead of code snippets for course translation blocks.
- Teach the learner how to spot what is wrong, why it matters, and what evidence supports that conclusion.

### 4. Decide build path

Use the skill’s path selection, with a bias toward problem-led structure:

- Simple source/topic: sequential build, usually 4-5 modules.
- Complex folder/source set: module briefs first, then parallel/partitioned writing, usually 5-6 modules.

For problem-focused courses, prefer modules that each center a distinct problem category discovered in the source.

### 5. Name the course

Derive the course slug from the topic, then append `-problems-course`.

Examples:

- `stocks-in-the-future` → `stocks-in-the-future-problems-course`
- `raccoon-task-instructions` → `raccoon-task-instructions-problems-course`

If the source already has a clean slug, reuse it as the base and add the `-problems-course` suffix.

### 6. Output location

Create course outputs under this workflow’s output directory:

```text
workflows/create-course-from-problems/output/<course-slug>/
```

The course directory should follow the skill’s required structure:

```text
<course-slug>/
  styles.css
  main.js
  _base.html
  _footer.html
  build.sh
  briefs/          # if complex
  modules/
    01-*.html
    02-*.html
  index.html       # generated by build.sh
```

### 7. Course requirements

Follow the skill’s mandatory requirements, adapted for problem discovery:

- 4-6 modules unless the source genuinely requires more
- each module must teach one concrete problem lens or failure mode from the source
- at least one translation block per module
  - codebase: code ↔ plain English problem interpretation
  - non-code: source excerpt/rule/workflow step ↔ plain-English meaning, impact, or fix direction
- at least one quiz per module
- at least one group chat animation across the course
- at least one message/data/process flow animation across the course
- glossary tooltips on first use of technical/domain terms per module
- warm, distinctive design using the skill’s design system

## Outputs

Write the complete generated course directory to:

```text
workflows/create-course-from-problems/output/<course-slug>/
```

Also write a short build note:

```text
workflows/create-course-from-problems/output/<course-slug>/BUILD-NOTES.md
```

The build note should include:

- source path(s) used
- mode: codebase / nominal folder / file-topic
- module list
- whether module briefs were used
- build command run
- final course path
- any limitations or unresolved source issues

## Verify

Before finishing, verify that:

- the `codebase-to-course` skill instructions were followed
- problem-focused adaptations are explicit and source-grounded
- prebuilt `styles.css`, `main.js`, `_footer.html`, and `build.sh` were copied, not regenerated
- module files contain only `<section class="module" ...>` content
- `index.html` was generated by running `build.sh`
- every module has a translation block and quiz
- the course contains at least one group chat animation and at least one process/data flow animation
- the output is under `workflows/create-course-from-problems/output/`
