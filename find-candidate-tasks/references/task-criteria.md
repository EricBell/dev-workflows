# Task Evaluation Criteria for LiveCompare

This file defines the detailed criteria for evaluating whether a candidate task meets LiveCompare standards. Use this as a checklist during Stage 2 (Task Evaluation).

---

## Core Criteria (All Must Pass)

### 1. Real Work
- [ ] The task represents work maintainers would actually want done
- [ ] It's a feature, bug fix, refactor, or tests - not a toy task
- [ ] Not "build me an app from an empty folder"
- [ ] Addresses a genuine pain point or limitation in the current codebase

### 2. Checkable (Verifiable Completion)
- [ ] Has a specific command a reviewer can run
- [ ] Has a specific expected output/result the reviewer should see
- [ ] "Check that the feature works" is NOT sufficient
- [ ] Must include setup commands, exact verification commands, and expected results
- [ ] Example of GOOD: "Run `python3 manage.py test` and verify 0 failures. Open `http://localhost:8000/project/1/tasks/` and verify due date column shows with past dates highlighted."

### 3. Big Enough (Several Hours, Many Turns)
- [ ] Will take at least 2-3 hours of work with the AI model
- [ ] Requires many conversation turns (not a 1-2 turn fix)
- [ ] Involves multiple files/components
- [ ] Requires exploration, implementation, testing, and iteration
- [ ] NOT a one-line fix or simple config change
- [ ] Can span multiple days if needed

### 4. Not Already Solved Publicly
- [ ] Checked GitHub issues for existing solutions
- [ ] Checked GitHub PRs (open and closed) for existing solutions
- [ ] Checked recent commits for related fixes
- [ ] If a solution exists, the task is disqualified

### 5. Safe
- [ ] No passwords, API keys, or secrets involved
- [ ] No engineered traps or adversarial setups
- [ ] Doesn't require the model to look things up online
- [ ] Doesn't require external services not in the repo
- [ ] No personal data

---

## Scoring Rubric (0-5 per criterion)

| Score | Real | Checkable | Big Enough | Not Solved | Safe |
|-------|------|-----------|------------|------------|------|
| 5 | Core maintainer priority, high impact | Exact commands + expected output documented | 4+ hours, 10+ turns, multi-area | Verified no issue/PR/commit exists | Zero concerns |
| 4 | Clear maintainer interest | Good commands, minor gaps in expected output | 3+ hours, 8+ turns | Checked issues/PRs, likely clear | Minor concerns, manageable |
| 3 | Plausible maintainer interest | Basic commands, some ambiguity | 2+ hours, 5+ turns | Quick check done, seems clear | Some concerns, need care |
| 2 | Nice-to-have only | Vague verification | 1-2 hours, few turns | Not thoroughly checked | Notable concerns |
| 1 | Toy/academic only | "Check it works" only | <1 hour, 1-2 turns | Known solution exists | Significant concerns |
| 0 | Not real work | Not checkable | Trivial | Already solved | Unsafe |

**Minimum passing score: 3 on ALL criteria** (total ≥ 15)

---

## Task Brief Quality Standards (from project-rules.md)

Each candidate that passes scoring must have a Task Brief with:

### Task Description (2-3+ sentences)
- **Weak**: "Fix the script."
- **Strong**: "sync_photos.py copies every file again on each run. It should skip files already in the destination with the same size and time."

### How to Judge Completion (2-3+ sentences with commands)
- **Weak**: "None given" or "Check that it works"
- **Strong**: "In the repo folder, run `python3 sync_photos.py test_photos/ /tmp/backup/`. The first run should print '412 copied, 0 skipped'. Run the same command again. The second run should print '0 copied, 412 skipped' and finish in under 5 seconds."

### Other Notes for Reviewer (2-3+ sentences)
- **Weak**: "The goal of the task is to fix the sync script."
- **Strong**: "This repo is a small backup tool I run every night to copy photos from my laptop to a home server. Right now each run re-copies about 40 GB and takes over an hour, so I often skip backups. Skipping unchanged files is what makes nightly backups practical."

---

## Red Flags (Automatic Disqualification)

- [ ] Task is a toy, padded, or built from an empty folder
- [ ] No sign the code was run or checked
- [ ] Checks don't run or are meaningless
- [ ] Requires secrets, keys, or personal data
- [ ] "Build me an app from scratch" type task
- [ ] Single file change that takes <30 minutes
- [ ] Already has a merged PR solving it
- [ ] Depends on external services not in repo (e.g., live collab servers)

---

## Good Task Patterns by Repo

### NearBeach Patterns
- Adding columns to lists with filtering/sorting
- Form validation improvements
- Permission edge cases
- Search enhancements
- Test coverage for untested views

### erd-editor Patterns
- Theme-related visibility issues
- Canvas interaction improvements
- Import/export format support
- Keyboard shortcut additions
- Performance on large diagrams

### Excalidraw Patterns
- Tool behavior refinements
- Arrow/shape connection logic
- Export quality improvements
- Keyboard shortcuts for precision
- Accessibility improvements
- File format compatibility

---

## Evaluation Process

1. **List candidates** from Stage 1 repo analysis
2. **Score each** against all 5 criteria (0-5)
3. **Filter** to only those with ≥3 on all criteria
4. **Rank** by total score (max 25)
5. **Select top 3-5** for Task Brief generation
6. **Document rationale** for each score