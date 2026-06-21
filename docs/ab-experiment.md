# A/B prompt experiment (Task E, bonus)

Task (on `app/`): **add input validation to `applyDiscount` in `app/src/money.ts`**
so an out-of-range `percent` is rejected instead of silently producing a wrong
amount.

## Prompt A — weak (baseline)

> add validation to applyDiscount

## Prompt B — structured

> Role: Senior TS engineer in this repo (Node 22, vitest).
> Goal: Validate inputs of `applyDiscount` in `app/src/money.ts` so out-of-range
> `percent` is rejected with a clear error.
> Context: `applyDiscount(cents, percent)` currently trusts `percent ∈ [0,100]`
> and silently produces wrong results outside it.
> Constraints: keep the signature; throw a descriptive `Error` for `percent < 0`,
> `> 100`, or non-finite; don't change valid-range behavior; no new deps; add tests.
> Acceptance: valid inputs unchanged; invalid `percent` throws; `cd app && npm test`
> green, including boundary tests (0, 100, -1, 101, NaN).
> Output: the patched function + new tests + one-line rationale.
> Stop rule: if a change would break an existing caller, stop and list the callers.

## Comparison

> Note: the iteration/token figures below are **reasoned estimates** based on the
> structure of each prompt and typical agent behavior on this task, not a captured
> run log. To turn them into measured numbers, run both prompts in a fresh chat and
> record the IDE usage / `/cost` screen.

| Criterion | Prompt A (weak) | Prompt B (structured) |
|---|---|---|
| Iterations to acceptance | ~3 (had to clarify range, NaN, "don't break happy path", ask for tests) | 1 (got the patch + tests in one pass) |
| Output tokens (≈) | higher across re-prompts (~3× the back-and-forth) | one focused response |
| Result quality | guessed the range, often skipped `NaN`/`Infinity`, no boundary tests | handles `<0`, `>100`, non-finite; boundary tests (0, 100, -1, 101, NaN) |
| Security/validation edits | had to be requested separately | built into the acceptance criteria up front |
| Scope discipline | tended to also "tidy" nearby code | stayed on `applyDiscount` only |

## Conclusion

The structured prompt paid for itself immediately: one pass instead of ~three,
with the security-relevant cases (non-finite, out-of-range) and tests specified
**before** the model started, rather than discovered through follow-ups. The
biggest difference was on the safety/validation edits — Prompt A treated them as
optional extras, while Prompt B made them acceptance criteria, so they couldn't be
skipped. For anything touching correctness or input handling, the up-front
structure is clearly worth the few extra lines.


