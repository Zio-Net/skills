---
name: docs-consistency-review
description: Review project documentation for contradictions with current code, configuration, tests, and other documents. Use when checking whether docs are accurate, validating a plan or design against repository evidence, or finding stale terminology, flows, assumptions, links, and missing documentation updates.
---

# Documentation Consistency Review

Review documentation against repository evidence. Report findings before proposing edits.

## Workflow

1. Read repository guidance and documentation indexes relevant to the requested scope.
2. Identify the documents and claims being reviewed. If no scope is given, select the smallest relevant set instead of reading the entire repository.
3. Classify sources before comparing them:
   - Accepted product or architecture documents describe intended behavior.
   - Code, configuration, and tests describe current implementation.
   - Plans and drafts describe proposed behavior.
   - Generated documents reflect their source and should not be treated as independent truth.
4. Trace meaningful claims to concrete evidence in related docs, code, configuration, tests, schemas, or commands.
5. Check terminology, behavior, examples, paths, links, and stated defaults for inconsistencies.
6. Distinguish confirmed inconsistencies from unresolved product or architecture decisions. Do not silently assume that code is correct when it conflicts with an accepted document.
7. Report findings first. For each finding, include:
   - Type: contradiction, stale information, ambiguity, or missing coverage.
   - Document location.
   - Conflicting or missing evidence.
   - Practical impact.
   - Recommended resolution.
8. If no findings remain, state what was checked and any important uncertainty or unverified runtime behavior.

## Editing

Keep the review read-only unless the user asks for changes. When editing is requested, make the smallest necessary documentation changes, preserve established terminology, and validate affected links, references, and commands when practical.
