# Compact PR handoff

Inspect the target repository's PR instructions and templates first. Preserve
mandatory first lines, issue links, headings, and required metadata. Repository
requirements override the generic section order below.

Render only non-empty sections and keep the complete handoff at or below 180
words unless the user explicitly asks for more or mandatory repository content
makes the limit impossible.

## Summary

Start with one plain-language sentence stating what changes for the user or
system. Follow with a short paragraph explaining how it works and any important
compatibility or no-data behavior. When clearer, replace that paragraph with
one compact arrow flow or up to three bullets. Do not narrate the review
process.

## Key decisions

Use no more than three bullets. Include only decisions a reviewer needs in
order to understand the implementation or its tradeoffs.

## Deliberate non-goals and accepted risks

Use no more than two bullets and omit the section when empty. Distinguish an
intentional scope boundary from unfinished work.

## Validation

Use one compact line naming the meaningful checks and any important conditional
gap.

## Reviewer focus

Use one compact line directing human attention to the highest-value remaining
question.

Do not include the lens agenda, resolved findings, internal dispositions,
worker identities, or a chronological review log.
