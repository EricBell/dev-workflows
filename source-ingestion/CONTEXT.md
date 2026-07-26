# Workflow: Source File Ingestion

## Purpose
Read one or more standalone source files from `sources/`, digest their contents, and produce a concise, reusable ingestion artifact for downstream workflows.

This workflow is invoked when the user asks to read, ingest, digest, summarize, or prepare context from one or more files in `sources/`.

## Inputs

| Layer | Path | Required | Notes |
| --- | --- | --- | --- |
| Layer 4 working | `sources/<requested-file>` | Yes | The user may request one file or multiple files. Prefer Markdown/JSON/text files. Do not read zip files unless explicitly instructed in a separate workflow. |
| Layer 3 reference | `workflows/source-ingestion/references/ingestion-format.md` | Yes | Defines the output format for the ingestion artifact. |

## Process

1. Confirm the requested file(s) exist under `sources/`.
2. Do not read or inspect `.zip` files for this workflow.
3. Read each requested standalone file completely when feasible.
   - For long files, read in chunks until the full file has been covered.
   - If a binary source exists alongside a converted `.md`, prefer the `.md` file unless the user explicitly asks for the binary.
4. Digest the material into an ingestion artifact that preserves:
   - document purpose
   - key entities, systems, policies, roles, and workflows
   - definitions and terminology
   - operational rules or constraints
   - unresolved ambiguities or follow-up questions
   - useful source anchors/section names when available
5. If multiple files are requested, include both per-file digests and a cross-file synthesis.
6. Write the result to `workflows/source-ingestion/output/` using a descriptive filename.

## Outputs

Write one Markdown file to `output/`:

```text
workflows/source-ingestion/output/<source-name-or-topic>-ingestion.md
```

For multiple files, use:

```text
workflows/source-ingestion/output/multi-source-ingestion.md
```

## Verify

Before finishing, verify that:

- every requested non-zip source file was read or explicitly identified as unavailable/unreadable
- no zip files were read or inspected
- the output artifact names all ingested files
- the digest separates source facts from interpretation
- ambiguities and missing context are listed explicitly
