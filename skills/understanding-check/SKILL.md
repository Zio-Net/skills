---
name: understanding-check
description: Check how well a user understood a PR, report, document, or topic they studied. Use when they want to test their own understanding through adaptive questions grounded in the material.
---

# Understanding Check

Help the user find out what they understood, what they missed, and what they can explain using evidence from the material. Treat this as a conversation, not a fixed quiz.

## Establish the material

Identify the material and the user's intended scope from the conversation. Read the accessible source before asking substantive questions. For a PR, inspect the change and enough surrounding context to understand its intent and consequences. If the material is unavailable or the scope would change what counts as a correct answer, ask the user for the source or boundary first. Do not imply that you checked material you could not inspect.

Form a private coverage map from broad to specific: (1) the central purpose, problem, or claim; (2) the main parts and how they fit together; (3) why the approach works, including decisions, evidence, and trade-offs; (4) consequential details, assumptions, limits, and failure cases. Distinguish what the source establishes from what you infer. Prioritize details that matter to understanding over names, line numbers, and incidental facts.

## Ask and adapt

Ask **one question at a time**, then wait for the answer. Start at the broadest level the user has not yet demonstrated: for a PR, “How does this change solve its central problem?” Work down through the main parts, their relationships, the mechanism, and consequential details. When the material has multiple substantive parts, prefer questions whose answers connect them; ask about one part alone to establish a missing prerequisite. Ask for a causal explanation or prediction when the source supports one: “Why does the safeguard in the validation section follow from the limitation described in the design?” or “What would break if that safeguard were removed?” Keep each turn to one clear question, rather than a bundle of prompts. Include enough context to identify the issue without supplying the answer.

After each answer, compare it with the source. Give brief, specific feedback on what the answer establishes and where it is incomplete or mistaken. Do not reveal a missing detail that the next question is meant to test. Do not equate different wording with misunderstanding. If the answer is ambiguous, ask one clarifying question. If it is partial, use the next question to probe the consequential gap. If a point is well understood, move to another important part or a harder connection. If the user remains stuck, explain the relevant point with source evidence and continue; do not keep rephrasing the same question indefinitely.

Track which levels, parts, and connections have actually been demonstrated. Descend when the user can explain the current level; if the answer exposes a gap, probe or explain that gap before asking about dependent details. After examining details, ask a synthesis question that connects them back to the main idea. Vary questions across important caveats and implementation or evidence details. A plausible answer that the source does not establish is a prompt to check the evidence, not proof of understanding. Do not ask trick questions or test facts absent from the agreed material.

## Finish

Within the agreed scope, continue until the user's answers demonstrate the central idea, each major part's role, at least one causal connection between parts, and the consequential details or limitations, or the user asks to stop. Summarize demonstrated strengths, unresolved gaps, and any limits imposed by the source or the number of answers. Point to the relevant parts of the material when practical. Offer a next step for the gaps. Give a score only if the user asks for one, and explain what it measures.
