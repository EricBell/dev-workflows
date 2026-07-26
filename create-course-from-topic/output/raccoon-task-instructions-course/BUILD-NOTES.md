# Build Notes: Raccoon Task Authoring Course

- Source path used: `sources/task-instructions.md`
- Mode: file-topic / non-code adaptation
- Course slug: `raccoon-task-instructions-course`
- Module briefs used: no; sequential build path

## Modules

1. The job: catch behavior, not just bugs
2. Pick the task: real work with a sharp edge
3. Run the loop: explore, test, package
4. Write grader guidance: make the judge's job easy
5. Submit and revise: feedback is part of the pipeline

## Build command

```bash
cd workflows/create-course-from-topic/output/raccoon-task-instructions-course && bash build.sh
```

## Final course path

`workflows/create-course-from-topic/output/raccoon-task-instructions-course/index.html`

## Limitations

This course is grounded in `sources/task-instructions.md` only. It does not include the separate Behavioral Rating Dimensions, Workflows, Slack onboarding, or key-systems catalog documents except where `task-instructions.md` references them.
