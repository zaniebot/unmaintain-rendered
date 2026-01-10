```yaml
number: 22382
title: "Add a `fast-test` profile"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/fast-build
created_at: 2026-01-05T01:51:48Z
updated_at: 2026-01-07T12:57:00Z
url: https://github.com/astral-sh/ruff/pull/22382
synced_at: 2026-01-10T16:30:32Z
```

# Add a `fast-test` profile

---

_Pull request opened by @charliermarsh on 2026-01-05 01:51_

## Summary

We use this profile in uv to create success, as an optimization for the iterative test loop. We include `opt-level=1` because it ends up being "worth it" for testing (empirically), even though it means the build is actually a big slower than `dev` (if you remove `opt-level=1`, clean compile is about 22% faster than `dev`).

Here are some benchmarks I generated with Claude -- the main motivator here is the incremental testing for `ty_python_semantic` which is 2.4x faster:

### `ty_python_semantic`

Full test suite (471 tests):
| Scenario    | dev   | fast-test | Improvement |
|-------------|-------|------------|-------------|
| Clean       | 53s   | 49s        | 8% faster   |
| Incremental | 17.8s | 6.8s       | 2.4x faster |

Single test:
| Scenario    | dev   | fast-test | Improvement |
|-------------|-------|------------|-------------|
| Clean       | 42.5s | 55.3s      | 30% slower  |
| Incremental | 6.5s  | 6.1s       | ~same       |

### `ruff_linter`

Full test suite (2622 tests):
| Scenario    | dev   | fast-test | Improvement |
|-------------|-------|------------|-------------|
| Clean       | 31s   | 41s        | 32% slower  |
| Incremental | 11.9s | 10.5s      | 12% faster  |

Single test:
| Scenario    | dev  | fast-test | Improvement |
|-------------|------|------------|-------------|
| Clean       | 26s  | 36.5s      | 40% slower  |
| Incremental | 4.5s | 5.5s       | 22% slower  |


---

_Renamed from "Add a fast-build profile" to "Add a `fast-build` profile" by @charliermarsh on 2026-01-05 01:51_

---

_Label `internal` added by @charliermarsh on 2026-01-05 01:51_

---

_Marked ready for review by @charliermarsh on 2026-01-05 01:51_

---

_Comment by @astral-sh-bot[bot] on 2026-01-05 02:03_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @ibraheemdev on 2026-01-05 03:45_

Might be worth it to test a higher opt-level on the salsa dependency specifically.

---

_Review comment by @MichaReiser on `Cargo.toml`:343 on 2026-01-05 07:27_

I would very much prefer keeping `debug` on or debug assertions will only hit once you run the normal dev profile. I also wonder if it's necessary to strip `debuginfo`. It will make backtraces less useful

---

_@MichaReiser reviewed on 2026-01-05 07:29_

> Might be worth it to test a higher opt-level on the salsa dependency specifically.

We already set opt-level=3 for salsa, but you're right, we only do this for dev builds

>  We include opt-level=1 because it ends up being "worth it" for testing (empirically), even though it means the build is actually a big slower than dev

What are the numbers in your PR summary. Is this end-to-end compile and test execution time? 

I also find the name confusing. It suggests to me that it is a faster build when it's really about faster execution times. Maybe `dev-optimized`

---

_@AlexWaygood reviewed on 2026-01-05 07:56_

---

_Review comment by @AlexWaygood on `Cargo.toml`:343 on 2026-01-05 07:56_

I agree that debug assertions being on make it much easier to pin down the causes of bugs in ty

---

_Comment by @charliermarsh on 2026-01-05 12:45_

> What are the numbers in your PR summary. Is this end-to-end compile and test execution time?

Yes, end-to-end time to build and run the tests.

---

_Renamed from "Add a `fast-build` profile" to "Add a `fast-test` profile" by @charliermarsh on 2026-01-05 17:11_

---

_@MichaReiser approved on 2026-01-05 19:23_

---

_Merged by @charliermarsh on 2026-01-05 19:35_

---

_Closed by @charliermarsh on 2026-01-05 19:35_

---

_Branch deleted on 2026-01-05 19:35_

---

_Comment by @AlexWaygood on 2026-01-07 11:43_

If it's generally fast to run the tests using `--profile=fast-test`, should we set that flag here in mdtest.py?

https://github.com/astral-sh/ruff/blob/3b7a5e4de81bb1eb594962894224cdacf99c5465/crates/ty_python_semantic/mdtest.py#L59-L70

(I usually run tests locally by running mdtest.py in an in-editor terminal, as it's by far the easiest way to make incremental updates to mdtests -- you can jump directly to failing mdtest lines, and failing snapshots don't block other mdtest assertions from being executed, the snapshots are just automatically updated behind the scenes when the script runs.)

---

_Comment by @MichaReiser on 2026-01-07 12:57_

It's a tradeoff between runtime and compile times. 

---
