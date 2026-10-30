# Stage 3: Task Brief Generation

## Purpose
Generate detailed, ready-to-use Task Brief materials for the top candidates from Stage 2, following the "Strong" examples from project-rules.md.

## Inputs

| Layer | Path | Required | Notes |
| --- | --- | --- | --- |
| Layer 3 reference | `sources/project-rules.md` | Yes | Strong examples for Task Brief fields |
| Layer 3 reference | `workflows/find-candidate-tasks/references/repo-profiles.md` | Yes | Setup commands for "How to judge completion" |
| Layer 4 working | `stages/02_task-evaluation/output/task-evaluation-<timestamp>.md` | Yes | Selected candidates with scores |

## Process

### 1. Load Selected Candidates
Read the task-evaluation output and extract the selected candidates.

### 2. Generate Task Brief for Each Candidate
For each selected candidate, create a complete Task Brief with three fields:

#### Task Description (2-3+ sentences)
Following the Strong example pattern:
- State the specific problem
- State what should happen instead
- Be concrete and specific

#### How to Judge Completion (2-3+ sentences with exact commands)
Following the Strong example pattern:
- Setup commands (from repo-profiles.md)
- Exact verification command(s) to run
- Exact expected output/result
- What the reviewer should see in the UI (if applicable)
- Test command and expected result (e.g., "pytest should report 0 failures, was 3 before")

#### Other Notes for Reviewer (2-3+ sentences)
Following the Strong example pattern:
- What the project is
- Why this task matters
- What problem it solves or what is limited today
- Big picture context the reviewer won't know

### 3. Include Practical Details
For each brief, also include:
- **Setup Commands**: Exact commands to get the repo running in Sandbox
- **Estimated Time**: e.g., "3-4 hours", "4-6 hours"
- **Complexity**: e.g., "Medium - touches 3-4 files, requires UI and backend changes"
- **Key Files Likely Involved**: Based on codebase exploration
- **Potential Challenges**: What might be tricky

## Outputs

Write to `stages/03_task-brief-generation/output/task-briefs-<timestamp>.md`:

```markdown
# Task Briefs for LiveCompare

## Candidate 1: <title>
**Score**: X/25 | **Estimated Time**: X hours | **Complexity**: X

### Task Description
<2-3+ sentences, Strong style>

### How to Judge Completion
<2-3+ sentences with exact commands and expected results, Strong style>

### Other Notes for Reviewer
<2-3+ sentences with project context, why it matters, Strong style>

### Practical Details
- **Setup Commands**:
  ```
  <exact commands from repo-profiles.md>
  ```
- **Key Files Likely Involved**:
  - <file1>
  - <file2>
- **Potential Challenges**:
  - <challenge1>
  - <challenge2>

---

## Candidate 2: <title>
...

---

## Candidate 3: <title>
...
```

## Verify

Before finishing, verify that:
- Each selected candidate has a complete Task Brief
- All three fields follow the "Strong" examples from project-rules.md (2-3+ sentences each)
- Setup commands are accurate for the repo
- Completion criteria are specific and runnable
- Reviewer notes provide genuine context (not just restating the task)
- Output file is in the stage's output directory