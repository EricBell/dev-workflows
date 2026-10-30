# Stage 4: User Selection

## Purpose
Present the evaluated candidates with Task Briefs to the user for final selection. Capture the user's choice and any notes.

## Inputs

| Layer | Path | Required | Notes |
| --- | --- | --- | --- |
| Layer 4 working | `stages/03_task-brief-generation/output/task-briefs-<timestamp>.md` | Yes | Complete Task Briefs for each candidate |
| Layer 4 working | User's selection | Yes | User chooses one task to pursue |

## Process

### 1. Present Candidates to User
Display a summary of each candidate:
- Title and score
- Estimated time and complexity
- One-line summary of the task
- Key highlight from Task Description

### 2. Get User Selection
Ask the user to select one candidate by number/title.

### 3. Capture User Notes
Ask the user for any notes on their selection:
- Why they chose this task
- Any modifications they want to make to the Task Brief
- Any concerns or specific angles they want to pursue

### 4. Prepare Final Output
Create the final selected task document with:
- The complete Task Brief for the chosen task
- User's notes and modifications
- Ready to copy into LiveCompare

## Outputs

Write to `stages/04_user-selection/output/selected-task-<timestamp>.md`:

```markdown
# Selected Task for LiveCompare

## User Selection
- **Chosen**: <Candidate title>
- **Score**: X/25
- **Selection Date**: <timestamp>
- **User Notes**: <any notes from user>

## Complete Task Brief (Ready for LiveCompare)

### Task Description
<from Stage 3, with any user modifications>

### How to Judge Completion
<from Stage 3, with any user modifications>

### Other Notes for Reviewer
<from Stage 3, with any user modifications>

## Practical Details
- **Setup Commands**:
  ```
  <from Stage 3>
  ```
- **Key Files Likely Involved**:
  <from Stage 3>
- **Potential Challenges**:
  <from Stage 3>

## Next Steps for User
1. Copy the Task Brief above into LiveCompare (Task Description, How to Judge Completion, Other Notes for Reviewer)
2. Download the repo zip from the approved repos table in project-rules.md
3. Mount the archive in LiveCompare before your first message
4. Use the File Source URL from the approved repos table
5. Start the conversation with the model using the Task Description as your first message
```

## Verify

Before finishing, verify that:
- User made a clear selection
- User notes were captured
- Final document contains the complete, ready-to-use Task Brief
- Any user modifications are incorporated
- Output file is in the stage's output directory