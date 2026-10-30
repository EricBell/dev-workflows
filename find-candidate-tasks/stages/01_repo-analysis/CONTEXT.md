# Stage 1: Repo Analysis

## Purpose
Explore the user-specified repository (via git URL) to identify 3-5 concrete task opportunities in the designated good task areas from project-rules.md.

## Inputs

| Layer | Path | Required | Notes |
| --- | --- | --- | --- |
| Layer 3 reference | `sources/project-rules.md` | Yes | Good task areas per repo |
| Layer 3 reference | `workflows/find-candidate-tasks/references/repo-profiles.md` | Yes | Repo profiles with setup commands and good areas |
| Layer 4 working | User's git URL | Yes | The repository to analyze |

## Process

### 1. Clone and Explore the Repository
- Clone the user-specified git URL
- Explore the codebase structure focusing on the good task areas from repo-profiles.md
- Understand the project structure, main components, and how it runs

### 2. Identify Task Opportunities
For each good task area listed in repo-profiles.md for this repo:
- Look for specific problems, limitations, or missing features
- Focus on areas that would be valuable to maintainers
- Identify 3-5 concrete, specific task opportunities

### 3. Check for Existing Solutions
For each candidate task:
- Search GitHub issues (via web search or user-provided info) for existing reports
- Search GitHub PRs (open and closed) for existing solutions
- Check recent commits for related fixes
- Document findings

### 4. Document Each Candidate
For each task opportunity, document:
- **Area**: Which good task area it falls under
- **Specific Problem**: What exactly is broken, missing, or could be improved
- **Why It Matters**: What problem it solves for users/maintainers
- **How to Judge Completion**: Specific commands and expected results (preliminary)
- **Existing Solution Check**: What you found in issues/PRs/commits

## Outputs

Write to `stages/01_repo-analysis/output/repo-analysis-<timestamp>.md`:

```markdown
# Repo Analysis: <repo-name> (<git-url>)

## Repository Overview
- Language/framework:
- Key directories:
- How to run:

## Good Task Areas (from profile)
- Area 1:
- Area 2:
- ...

## Candidate Tasks

### Candidate 1: <title>
- **Area**: 
- **Specific Problem**: 
- **Why It Matters**: 
- **Preliminary Completion Criteria**: 
- **Existing Solution Check**: 

### Candidate 2: <title>
...

### Candidate 3: <title>
...

### Candidate 4: <title>
...

### Candidate 5: <title>
...

## Notes
- Any observations about the codebase
- Areas that seemed particularly promising or problematic
```

## Verify

Before finishing, verify that:
- The repository was cloned and explored
- Good task areas from the profile were used as starting points
- 3-5 concrete candidates were identified
- Each candidate has a preliminary completion criteria
- Existing solution check was performed for each
- Output file is in the stage's output directory