```yaml
number: 13146
title: "Expand docs for `ASYNC109`"
type: pull_request
state: merged
author: jamesbraza
labels:
  - documentation
assignees: []
merged: true
base: main
head: better-async109-docs
created_at: 2024-08-29T05:14:00Z
updated_at: 2024-09-01T21:47:18Z
url: https://github.com/astral-sh/ruff/pull/13146
synced_at: 2026-01-10T21:38:32Z
```

# Expand docs for `ASYNC109`

---

_Pull request opened by @jamesbraza on 2024-08-29 05:14_

## Summary

Documenting when to use or not use ASYNC109 a bit more.

Closes https://github.com/astral-sh/ruff/issues/12353

## Test Plan

It wasn't tested


---

_Comment by @github-actions[bot] on 2024-08-29 05:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_async/rules/async_function_with_timeout.rs`:15 on 2024-08-29 09:55_

Hmm, this isn't quite true as you currently have it. If the module imports `trio` or `anyio`, it's still applied on Python <3.11. It's only ignored on Python <3.11 if we detect that the user is using `asyncio` as their async framework.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_async/rules/async_function_with_timeout.rs`:25 on 2024-08-29 10:07_

"If the rule has false positives, you can disable it" feels like quite general advice that could be applied to any rule ;) I'm not sure we need to say that; it's good to keep the docs concise where possible.

Maybe something like this?

```suggestion
/// For functions that wrap `asyncio.timeout`,
/// false positives from this rule can be avoided by using
/// a different parameter name.
```

---

_@AlexWaygood reviewed on 2024-08-29 10:15_

Thanks! I agree the docs for this function can be improved, and I think this is a step in the right direction overall. However, I'm not sure the PR fully addresses @jakkdl's comments in https://github.com/astral-sh/ruff/issues/12353#issuecomment-2245666064 and https://github.com/astral-sh/ruff/issues/12353#issuecomment-2296575231. It would be great if the rule's docs could attempt to give a proper explanation of why the rule exists in the first place (to help enforce opinionated design decisions relating to a broader philosophy of how structured concurrency works), probably linking to https://vorpus.org/blog/some-thoughts-on-asynchronous-api-design-in-a-post-asyncawait-world/#timeouts-and-cancellation.

I guess those updates don't necessarily need to be made in this PR, but I don't think this PR is sufficient to close out #12353 as it currently stands

---

_Label `documentation` added by @AlexWaygood on 2024-08-29 10:19_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-08-30 07:44_

---

_@jamesbraza reviewed on 2024-09-01 06:08_

---

_Review comment by @jamesbraza on `crates/ruff_linter/src/rules/flake8_async/rules/async_function_with_timeout.rs`:15 on 2024-09-01 06:08_

Oh nice catch here, yeah I see what you're saying. I just updated the wording for that

---

_@jamesbraza reviewed on 2024-09-01 06:26_

---

_Review comment by @jamesbraza on `crates/ruff_linter/src/rules/flake8_async/rules/async_function_with_timeout.rs`:25 on 2024-09-01 06:26_

Thanks for the great suggestion, implemented

---

_Comment by @jamesbraza on 2024-09-01 06:27_

Alright, I just added a small blurb mentioning structured concurrency and linking that blog post 👌 

Thanks for the detailed and thoughtful review

---

_Review requested from @AlexWaygood by @jamesbraza on 2024-09-01 06:27_

---

_@AlexWaygood approved on 2024-09-01 10:07_

Thanks!

---

_Renamed from "Expanding `ASYNC109` docs" to "Expand docs for `ASYNC109`" by @AlexWaygood on 2024-09-01 10:12_

---

_Merged by @AlexWaygood on 2024-09-01 10:16_

---

_Closed by @AlexWaygood on 2024-09-01 10:16_

---

_Comment by @codspeed-hq[bot] on 2024-09-01 10:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jamesbraza:better-async109-docs)

### Merging #13146 will **degrade performances by 5.48%**

<sub>Comparing <code>jamesbraza:better-async109-docs</code> (7f61ec7) with <code>main</code> (52d8847)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/jamesbraza:better-async109-docs)._

### Benchmarks breakdown

|     | Benchmark | `main` | `jamesbraza:better-async109-docs` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 727.5 µs | 769.7 µs | -5.48% |


---

_Branch deleted on 2024-09-01 21:47_

---
