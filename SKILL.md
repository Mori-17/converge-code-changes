---
name: converge-code-changes
description: Converge code changes into one clean current-state implementation when fixing bugs, revising an earlier attempt after feedback, replacing or removing behavior, or cleaning patch residue. Use especially when a request says 不要、删掉、去掉、改回、清理、重做, or when a change might add fallback, compatibility branches, duplicate paths, or comments about rejected approaches.
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
- Removed UI or features leave no orphaned visual, structural, configuration, test, or documentation residue.
- Accepted behavior was not accidentally removed while cleaning its former patch structure.
- Unrelated user changes remain untouched.

Then run proportionate non-visual checks that exercise the resulting behavior and nearby regression surface.

## Report the result

Describe the final behavior, the root cause or invariant when relevant, and the verification performed. It is fine to tell the user that obsolete residue was removed; do not write that history back into the product. If a fallback or compatibility path remains, identify the present evidence that requires it.
