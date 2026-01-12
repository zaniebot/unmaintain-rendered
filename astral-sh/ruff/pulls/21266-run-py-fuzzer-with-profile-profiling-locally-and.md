```yaml
number: 21266
title: "Run py-fuzzer with `--profile=profiling` locally and in CI"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/fuzz-profile-profiling
created_at: 2025-11-03T21:27:37Z
updated_at: 2025-11-03T21:53:44Z
url: https://github.com/astral-sh/ruff/pull/21266
synced_at: 2026-01-12T15:57:20Z
```

# Run py-fuzzer with `--profile=profiling` locally and in CI

---

_@AlexWaygood_

## Summary

Following #20962 we're seeing the fuzzer take much longer in CI on seed 742 than on any other seed (https://github.com/astral-sh/ruff/pull/20962#issuecomment-3477968074). But this problem is only reproducible in debug builds, so this doesn't seem like an issue that's worth worrying much about.

This PR switches the fuzzer CI job to use `--profile=profiling`, and also makes this the new default if you run the fuzzer locally. This should mean that the build the fuzzer uses in CI has more similar performance characteristics to a release build. This in turn means we don't have to constantly wonder whether the fuzzer taking twice as long is indicative of a "real" issue or just an artifact of debug-build weirdness.

As well this, this also reduces the amount of time the fuzzer CI job takes (4min30s -> 3min40s) and reduces the amount of time we need to wait for the fuzzer job to start. (Prior to this PR, you had to wait for the `cargo-test-linux` job to finish before it would even start, because it reused the build uploaded from that job.)

## Test Plan

CI on this PR


---

_Label `ci` added by @AlexWaygood on 2025-11-03 21:27_

---

_Label `ty` added by @AlexWaygood on 2025-11-03 21:27_

---

_Marked ready for review by @AlexWaygood on 2025-11-03 21:48_

---

_Comment by @github-actions[bot] on 2025-11-03 21:51_

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

_@MichaReiser approved on 2025-11-03 21:53_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:679 on 2025-11-03 21:53_

this is all basically copied-and-pasted from https://github.com/astral-sh/ruff/blob/79a02711c12d5381a640c7b07639e058917a3234/.github/workflows/typing_conformance.yaml#L63-L78

and is also similar to what mypy_primer does.

---

_@AlexWaygood reviewed on 2025-11-03 21:53_

---

_Merged by @AlexWaygood on 2025-11-03 21:53_

---

_Closed by @AlexWaygood on 2025-11-03 21:53_

---

_Branch deleted on 2025-11-03 21:53_

---
