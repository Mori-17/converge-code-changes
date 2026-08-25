---
name: converge-code-changes
description: Use whenever a user explicitly names text, UI copy, modules, files, code, behaviors, or concepts to delete or remove. Also use for negative or subtractive constraints such as do not, no, only, avoid, 不要、不需要、删除、去掉、仅, and for revisions, cleanup, or bug fixes. Converge the result into one current-state implementation without residue, paraphrases, disclaimers, or duplicate paths.
---

# Converge Code Changes

Treat the conversation as deliberation and the repository as the current product. Implement the user's net current intent; do not preserve the path taken to reach it. The result should make sense to an engineer who has the repository but has never seen the conversation.

## Reconstruct the net contract

Before editing, derive four things from the latest request, relevant corrections, and current repository behavior:

1. The observable behavior that should exist when the task is complete.
2. Existing behavior and constraints that must remain intact.
3. The superseded behavior or mechanism that should disappear.
4. Evidence for any compatibility, fallback, or exceptional path that must remain.

Apply these distinctions:

- A later correction replaces a conflicting earlier choice. It does not create a negative requirement that must be encoded as `not-old-choice`.
- A rejected proposal is conversation history, not a product constraint.
- A reference, attachment, or example is evidence for the requested aspects, not a wholesale specification unless the user explicitly asks for faithful reproduction.
- “Clean/remove the patch” normally means remove workaround structure, rejected paths, and narration while preserving the accepted behavior. “Remove the feature” means remove the behavior end to end.
- “Replace the approach” keeps the intended outcome and replaces its mechanism. Do not interpret it as adding a second mechanism.
- “Revert” requires an identifiable target state. Do not infer a full behavioral revert merely from words such as “clean up.”

If two interpretations would materially change user-visible behavior and repository evidence cannot resolve them, ask one focused question before editing.

## Treat named deletion as a hard trigger

When the user explicitly lists, quotes, marks, or otherwise identifies items to delete, treat every named item as a required deletion target, not as an example, preference, or optional suggestion.

- Resolve each target to its exact occurrences, semantic equivalents on the affected surface, owning container, and directly owned dependency cone.
- If the same named text or concept appears more than once in scope, remove every occurrence.
- Remove the target together with layout, styles, handlers, assets, tests, documentation, and configuration that exist only for it.
- Do not leave an empty shell, placeholder, heading, separator, spacing rule, or navigation entry after its content is removed.
- Do not replace the target with a paraphrase, shorter disclaimer, renamed module, or another expression of the same excluded concept.
- Preserve unrelated current behavior and shared infrastructure that still has active consumers.
- Before reporting completion, account for every named target: removed, deliberately retained with present-tense evidence, or blocked by a specific constraint. Never silently skip an item.

## Implement exclusions as absence

A negative requirement normally defines what the final artifact must omit. It is not a request to create copy explaining the omission.

- “Do not add X” means X is absent from the implementation and interface. Do not add a “No X,” “X is not used,” or “Unlike X” label, note, badge, section, comment, or documentation entry.
- Do not promote implementation facts or prompt constraints into visible product copy. A fixed question set, local storage, absence of AI, non-diagnostic scoring, unsupported behavior, or a rejected technology does not need to be advertised merely because it was discussed while building the product.
- Do not create unsolicited assurance, methodology, limitation, privacy, age-gate, or “method and boundaries” blocks merely to prove that requirements were followed.
- When the user names text or modules to remove, treat those strings and concepts as tombstoned on the affected surface for the current task. Remove their containers and owned layout; do not paraphrase, shorten, rename, relocate, or replace them with another explanation of the same excluded concept.
- If the remaining experience needs copy, describe what the product currently does in direct positive terms. Do not define it through a catalog of things it is not.

Keep a disclosure only when the user explicitly requests it or current legal, safety, privacy, consent, or material-risk evidence requires users to see it. Make it minimal, factual, and located at the relevant decision point. Do not invent a compliance requirement; if omitting a possibly required disclosure would create material risk and evidence is unclear, ask before changing it.

## Fix the violated invariant at its source

Trace the failing value or behavior through its producer, transformation, and consumers. Prefer restoring the invariant where it first becomes false over masking the symptom downstream.

Do not add a fallback, default value, null guard, broad `try/catch`, retry, compatibility shim, feature flag, duplicate code path, or translation wrapper merely to make the immediate failure disappear. Keep such a mechanism only when a current contract requires it, supported by concrete evidence such as an external schema, real persisted data, a supported version boundary, an explicit product requirement, or a meaningful test.

When a boundary mechanism is genuinely required, make it a deliberate canonical boundary and describe the present constraint. Do not narrate the failed attempt that led to it.

## Replace instead of layering

When feedback changes an implementation choice, revise the existing implementation. Do not append another override, conditional, wrapper, style rule, or exception on top of it.

Sweep the dependency cone touched by the change:

- implementation, callers, imports, state, types, schemas, configuration, and feature flags;
- UI structure, selectors, pseudo-elements, separators, borders, glows, min-heights, and spacing owned by removed content;
- handlers, routes, assets, generated mappings, and localization entries;
- tests, fixtures, snapshots, examples, and documentation.

Remove artifacts that became obsolete because of this task, but do not expand into unrelated pre-existing cleanup. After changing a contract, update its producers and consumers so the repository has one canonical shape rather than old and new shapes joined by adapters.

## Keep repository prose current-state only

Delete or rewrite comments, documentation, names, and UI copy that describe the negotiation or edit history, including ideas such as:

- “previously,” “now,” “no longer,” “the old version,” or “changed from”;
- “the user did not want X” or “do not use the earlier approach”;
- temporary notes preserving a rejected attempt;
- commented-out code and obsolete TODOs;
- names such as `new`, `legacy`, `temp`, `without_x`, or `v2` when there is no real, current distinction.

Keep a comment only when it explains a non-obvious current invariant, external constraint, safety property, protocol rule, or unit assumption. Rewrite it so it stands on repository evidence alone.

Use this test: would the sentence still help a new engineer who sees only the current checkout? If not, remove it.

## Make tests converge too

Test the accepted behavior and the invariant that prevents regression. Do not preserve tests whose only purpose is to memorialize a rejected implementation or assert that conversation-specific alternatives are absent.

For a bug fix, prefer the smallest regression test that exposes the violated invariant through the real path. When feedback replaces an earlier choice, update or remove the earlier test instead of stacking contradictory tests.

## Run a convergence pass

Before final verification, inspect the complete diff and relevant touched files. Simplify again until all of these are true:

- Every changed line traces to the net current request or a required surviving constraint.
- The behavior has one canonical implementation path.
- No speculative fallback or compatibility layer remains.
- Comments, docs, tests, and names describe the current system rather than the change history.
- Negative constraints are realized as absence; no unrequested disclaimer, assurance block, or paraphrase of an excluded concept appears in user-facing copy.
- Removed UI or features leave no orphaned visual, structural, configuration, test, or documentation residue.
- Accepted behavior was not accidentally removed while cleaning its former patch structure.
- Unrelated user changes remain untouched.

Then run proportionate non-visual checks that exercise the resulting behavior and nearby regression surface.

## Report the result

Describe the final behavior, the root cause or invariant when relevant, and the verification performed. Summarize removals by category instead of repeating every forbidden phrase unless exact matching is useful evidence. It is fine to tell the user that obsolete residue was removed; do not write that history back into the product. If a fallback or compatibility path remains, identify the present evidence that requires it.
