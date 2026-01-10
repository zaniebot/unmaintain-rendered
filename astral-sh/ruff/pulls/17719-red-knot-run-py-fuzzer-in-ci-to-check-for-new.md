```yaml
number: 17719
title: "[red-knot] Run py-fuzzer in CI to check for new panics"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/panic-regrtest
created_at: 2025-04-29T18:07:43Z
updated_at: 2025-04-29T21:25:40Z
url: https://github.com/astral-sh/ruff/pull/17719
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Run py-fuzzer in CI to check for new panics

---

_Pull request opened by @AlexWaygood on 2025-04-29 18:07_

## Summary

This PR sets up a CI job that uses the py-fuzzer script to check whether a PR introduces new panics to red-knot.

The py-fuzzer script still finds lots of panics in red-knot generally (see https://github.com/astral-sh/ty/issues/228 for a tracking issue), so running it with the default arguments in CI would lead to a lot of red CI jobs. However, the py-fuzzer script _also_ has a `--only-new-bugs` CLI flag! If specified, it allows you to compare two binaries and check whether any _new_ panics have been introduced by the PR. If any have been introduced, the script fails; if there are no new panics relative to the baseline binary, however, the script exits with code 0.

The CI job here ends up looking like a cross between the `fuzz-parser` CI job in `ci.yaml` (which already runs the `py-fuzzer` script in CI for us, but which runs it with the `ruff` binary to fuzz the parser rather than red-knot) and the `ruff-ecosystem` CI job in `ci.yaml` (which, in a similar way to this new CI job, downloads two binaries and compares them to see if there's any differences).

The CI job is currently failing because there's no baseline red-knot binary for the job to download (this PR adds the CI job that will start uploading the red-knot binary for future CI jobs to download). I'm not totally sure how to address this -- I suppose I could split it up into two PRs, one that adds the job uploading the binary and then a second one that adds the job to actually use the binary? I'll wait for review on the general idea here before going ahead with that, though.

## Test Plan

I did the following:
- Built the binary on `main` with `--target-directory=target/main`
- Switched to a branch that had more panics and ran `cargo build -p red_knot`
- Ran the following command from my terminal and observed that the script only reported new panics that did not exist on `main`:

  ```
  uvx \
      --python="3.12" \
      --from=./python/py-fuzzer \
      fuzz \
      --test-executable="./target/debug/red_knot" \
      --baseline-executable="./target/main/debug/red_knot" \
      --only-new-bugs \
      --bin=red_knot \
      0-500
  ```


---

_Label `ci` added by @AlexWaygood on 2025-04-29 18:07_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-29 18:07_

---

_Comment by @github-actions[bot] on 2025-04-29 18:17_

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

_Marked ready for review by @AlexWaygood on 2025-04-29 18:43_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-29 18:43_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-29 18:43_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-04-29 18:43_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-29 18:43_

---

_Comment by @dhruvmanila on 2025-04-29 19:39_

> The CI job is currently failing because there's no baseline red-knot binary for the job to download (this PR adds the CI job that will start uploading the red-knot binary for future CI jobs to download). I'm not totally sure how to address this -- I suppose I could split it up into two PRs, one that adds the job uploading the binary and then a second one that adds the job to actually use the binary? I'll wait for review on the general idea here before going ahead with that, though.

Or, you could create a dummy PR with either an empty commit or a random change (if it's required) to trigger CI which is stacked on top of this PR.

---

_Closed by @AlexWaygood on 2025-04-29 20:04_

---

_Reopened by @AlexWaygood on 2025-04-29 20:04_

---

_Converted to draft by @AlexWaygood on 2025-04-29 20:15_

---

_Marked ready for review by @AlexWaygood on 2025-04-29 20:50_

---

_Comment by @AlexWaygood on 2025-04-29 20:51_

> Or, you could create a dummy PR with either an empty commit or a random change (if it's required) to trigger CI which is stacked on top of this PR.

I split this PR into two -- this change is now stacked on top of https://github.com/astral-sh/ruff/pull/17720 (so that there is a baseline binary to download), and CI is passing on the new job 🎉 https://github.com/astral-sh/ruff/actions/runs/14740981444/job/41378854533?pr=17719#step:6:539

---

_Review comment by @carljm on `.github/workflows/ci.yaml`:640 on 2025-04-29 20:56_

This is the name that shows up in the CI jobs list -- I think it should mention fuzzing, it's not clear otherwise what "check for new red-knot panics" actually means

---

_@carljm approved on 2025-04-29 20:56_

Thank you!

---

_@AlexWaygood reviewed on 2025-04-29 21:01_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:640 on 2025-04-29 21:01_

Maybe this?

```suggestion
    name: "Fuzz for new red-knot panics"
```

---

_@AlexWaygood reviewed on 2025-04-29 21:02_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:640 on 2025-04-29 21:02_

or

```suggestion
    name: "Check for new red-knot panics using py-fuzzer"
```

---

_@carljm reviewed on 2025-04-29 21:09_

---

_Review comment by @carljm on `.github/workflows/ci.yaml`:640 on 2025-04-29 21:09_

Either seems fine to me, up to you!

---

_Closed by @AlexWaygood on 2025-04-29 21:15_

---

_Reopened by @AlexWaygood on 2025-04-29 21:15_

---

_Review comment by @dhruvmanila on `.github/workflows/ci.yaml`:646 on 2025-04-29 21:16_

Yeah, and it's most useful to run it on a PR. We also don't run the py-fuzzer on parser on `main`.

---

_@dhruvmanila approved on 2025-04-29 21:16_

---

_Merged by @AlexWaygood on 2025-04-29 21:19_

---

_Closed by @AlexWaygood on 2025-04-29 21:19_

---

_Branch deleted on 2025-04-29 21:19_

---

_Comment by @codspeed-hq[bot] on 2025-04-29 21:21_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fpanic-regrtest)

### Merging astral-sh/ruff#17719 will **improve performances by 4.1%**

<sub>Comparing <code>alex/panic-regrtest</code> (8c563cc) with <code>main</code> (8c68d30)</sub>



### Summary

`⚡ 1` improvements  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` parser[numpy/ctypeslib.py] `` | 924.5 µs | 888 µs | +4.1% |


---

_Comment by @AlexWaygood on 2025-04-29 21:24_

> Merging astral-sh/ruff#17719 will **improve performances by 4.1%**

hmmmmm

---
