# Workflow: Find Candidate Tasks for LiveCompare

## Purpose
Discover and evaluate real coding tasks from a **user-specified approved repository** (NearBeach, erd-editor, or Excalidraw) that meet the LiveCompare project criteria: real work maintainers would want, checkable with commands, big enough for several hours of AI conversation, not already solved publicly, and safe.

This workflow produces a ranked list of candidate tasks with enough detail for the user to choose one and write a strong Task Brief for LiveCompare.

## Trigger
Use this workflow when the user wants to find a new task to work on in LiveCompare, or when they need fresh task candidates after completing a previous task.

## Inputs

| Layer | Path | Required | Notes |
| --- | --- | --- | --- |
| Layer 3 reference | `sources/project-rules.md` | Yes | Project rules defining what makes a good task, approved repos, and evaluation criteria |
| Layer 3 reference | `sources/ICM.md` | Yes | ICM methodology for workflow structure |
| Layer 3 reference | `workflows/find-candidate-tasks/references/repo-profiles.md` | Yes | Profiles of each approved repo with good task areas and setup commands |
| Layer 3 reference | `workflows/find-candidate-tasks/references/task-criteria.md` | Yes | Detailed criteria for evaluating task quality |
| Layer 4 working | User's specified repo | **Yes** | User must specify a git URL (e.g., https://github.com/nearbeach/NearBeach, https://github.com/dineug/erd-editor, https://github.com/excalidraw/excalidraw) |
| Layer 4 working | User's preferred task type (optional) | No | Feature, bug fix, refactor, tests, etc. |

## Process

### Stage 1: Repo Analysis (01_repo-analysis)
Analyze the **user-specified repository (via git URL)** to identify concrete task opportunities in the designated good task areas:
- Explore the codebase structure focusing on good task areas from project-rules.md
- Identify 3-5 specific, concrete task opportunities
- Check GitHub issues/PRs (via user-provided info or web search) to ensure not already solved
- Document each candidate with: area, specific problem, why it matters, how to judge completion

### Stage 2: Task Evaluation (02_task-evaluation)
Evaluate each candidate task against the LiveCompare criteria:
- **Real**: Is this work maintainers would actually want?
- **Checkable**: Can a reviewer run a command and see a specific expected result?
- **Big enough**: Will this take several hours and many AI conversation turns? (A couple of hours minimum)
- **Not already solved**: Confirmed no existing PR/issue solves it
- **Safe**: No secrets, no engineered traps, no external dependencies

Score each task and filter to top candidates.

### Stage 3: Task Brief Generation (03_task-brief-generation)
For the top 3-5 candidates, generate detailed Task Brief materials the user can copy into LiveCompare:
- Task description (2-3+ sentences)
- How to judge completion (specific commands, expected output)
- Other notes for reviewer (project context, why this matters, what problem it solves)
- Setup commands for the Sandbox
- Estimated time and complexity

### Stage 4: User Selection (04_user-selection)
Present the evaluated candidates to the user with scores and rationale. User selects one task to pursue.

## Outputs

Write to `workflows/find-candidate-tasks/output/`:

```
workflows/find-candidate-tasks/output/
  repo-analysis-<timestamp>.md      # Stage 1 output: raw task opportunities per repo
  task-evaluation-<timestamp>.md    # Stage 2 output: scored and filtered candidates
  task-briefs-<timestamp>.md        # Stage 3 output: ready-to-use Task Briefs
  selected-task-<timestamp>.md      # Stage 4 output: user's choice with notes
```

## Verify

Before finishing, verify that:
- The user-specified repo (git URL) was analyzed
- Each candidate task has concrete completion criteria with runnable commands
- No candidate is a toy task, padded task, or empty-folder task
- Task briefs follow the "Strong" examples from project-rules.md (2-3+ sentences each field)
- Output files are in the workflow's output directory