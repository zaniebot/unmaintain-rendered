```yaml
number: 17788
title: "[red-knot] Refactor: no mutability in call APIs"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/call-api-mutability
created_at: 2025-05-02T09:25:12Z
updated_at: 2025-05-02T13:40:16Z
url: https://github.com/astral-sh/ruff/pull/17788
synced_at: 2026-01-10T18:57:03Z
```

# [red-knot] Refactor: no mutability in call APIs

---

_Pull request opened by @sharkdp on 2025-05-02 09:25_

## Summary

Remove mutability in parameter types for a few functions such as `with_self` and `try_call`. I tried the `Rc`-approach with cheap cloning [suggest here](https://github.com/astral-sh/ruff/pull/17733#discussion_r2068722860) first, but it turns out we need a whole stack of prepended arguments (there can be [both `self` *and* `cls`](https://github.com/astral-sh/ruff/blob/3cf44e401a64658c17652cd3a17c897dc50261eb/crates/red_knot_python_semantic/resources/mdtest/call/constructor.md?plain=1#L113)), and we would need the same construct not just for `CallArguments` but also for `CallArgumentTypes`. At that point we're cloning `VecDeque`s anyway, so the overhead of cloning the whole `VecDeque` with all arguments didn't seem to justify the additional code complexity.

## Benchmarks

I see small performance *improvements* on this branch?! Maybe because we can run more things in parallel with less mutability?

![image](https://github.com/user-attachments/assets/0cb48838-27a1-4d2c-b4b5-650460ff2dc3)


---

_Label `red-knot` added by @sharkdp on 2025-05-02 09:25_

---

_Comment by @github-actions[bot] on 2025-05-02 09:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-05-02 09:46_

---

_Review requested from @carljm by @sharkdp on 2025-05-02 09:46_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-05-02 09:46_

---

_Review requested from @dcreager by @sharkdp on 2025-05-02 09:46_

---

_@AlexWaygood approved on 2025-05-02 10:49_

Nice, this seems cleaner for sure. Definitely worth it if perf is neutral/slightly better.

---

_Comment by @AlexWaygood on 2025-05-02 10:50_

Could also be worth running the `knot_benchmark` script on larger codebases like black, just to be sure that this doesn't regress performance on those, I suppose?

---

_Comment by @sharkdp on 2025-05-02 11:52_

Inconclusive (no statistically significant improvement/regression)

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `red_knot_main check` | 183.6 ± 8.2 | 170.2 | 197.8 | 1.01 ± 0.07 |
| `red_knot_feature check` | 182.4 ± 9.4 | 167.3 | 204.5 | 1.00 |


---

_Merged by @sharkdp on 2025-05-02 11:53_

---

_Closed by @sharkdp on 2025-05-02 11:53_

---

_Branch deleted on 2025-05-02 11:53_

---

_@dcreager reviewed on 2025-05-02 13:40_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:18 on 2025-05-02 13:40_

We don't need to clone when there's no `bound_self`.  Opened #17793 for that, and to combine the `clone`/`push_front` into a single `collect` call, which should avoid a double-copy when there _is_ a `bound_self`.  (Or really, a copy + immediate move)

---
