# Stage 2: Task Evaluation

## Purpose
Evaluate each candidate task from Stage 1 against the LiveCompare criteria using the scoring rubric in task-criteria.md. Filter to only tasks that pass all minimum thresholds.

## Inputs

| Layer | Path | Required | Notes |
| --- | --- | --- | --- |
| Layer 3 reference | `workflows/find-candidate-tasks/references/task-criteria.md` | Yes | Scoring rubric and evaluation standards |
| Layer 4 working | `stages/01_repo-analysis/output/repo-analysis-<timestamp>.md` | Yes | Candidate tasks from Stage 1 |

## Process

### 1. Load Candidates
Read the repo-analysis output file from Stage 1 and extract all candidate tasks.

### 2. Score Each Candidate
For each candidate, score 0-5 on all 5 criteria:
- **Real** (0-5)
- **Checkable** (0-5)
- **Big Enough** (0-5)
- **Not Already Solved** (0-5)
- **Safe** (0-5)

Use the detailed rubric in task-criteria.md. Be strict - this is for LiveCompare quality.

### 3. Apply Minimum Threshold
- **Minimum passing score: 3 on ALL criteria** (total ≥ 15)
- Any candidate scoring <3 on any criterion is automatically disqualified
- Document the rationale for each score

### 4. Rank and Select
- Rank passing candidates by total score (max 25)
- Select top 3-5 candidates for Task Brief generation
- If fewer than 3 pass, note this and proceed with what you have

### 5. Check Red Flags
Review each passing candidate against the Red Flags list in task-criteria.md. Disqualify any that trigger a red flag.

## Outputs

Write to `stages/02_task-evaluation/output/task-evaluation-<timestamp>.md`:

```markdown
# Task Evaluation Results

## Candidates Evaluated
Total from Stage 1: X

## Scoring Results

### Candidate 1: <title>
- **Real**: X/5 - <rationale>
- **Checkable**: X/5 - <rationale>
- **Big Enough**: X/5 - <rationale>
- **Not Already Solved**: X/5 - <rationale>
- **Safe**: X/5 - <rationale>
- **Total**: X/25
- **Pass**: Yes/No
- **Red Flags**: None / <flag>

### Candidate 2: <title>
...

## Selected for Task Brief Generation
1. <Candidate name> - Score: X/25
2. <Candidate name> - Score: X/25
3. <Candidate name> - Score: X/25
...

## Disqualified Candidates
- <Candidate name> - Reason: <which criterion failed or red flag>
...
```

## Verify

Before finishing, verify that:
- All candidates from Stage 1 were scored
- Scoring follows the rubric in task-criteria.md
- Minimum threshold (3 on all criteria) was applied
- Red flags were checked
- Top 3-5 passing candidates selected
- Output file is in the stage's output directory