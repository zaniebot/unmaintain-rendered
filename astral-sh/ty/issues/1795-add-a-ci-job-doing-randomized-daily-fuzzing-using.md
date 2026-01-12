```yaml
number: 1795
title: Add a CI job doing randomized daily fuzzing using the py-fuzzer script
type: issue
state: open
author: AlexWaygood
labels:
  - ci
  - testing
  - fuzzer
assignees: []
created_at: 2025-12-07T15:39:00Z
updated_at: 2025-12-08T17:44:58Z
url: https://github.com/astral-sh/ty/issues/1795
synced_at: 2026-01-12T15:54:25Z
```

# Add a CI job doing randomized daily fuzzing using the py-fuzzer script

---

_@AlexWaygood_

We currently have a CI job that runs the py-fuzzer script on every ty PR: https://github.com/astral-sh/ruff/blob/285d6410d3dcc0567fee1d72bc46bfe3008af36b/.github/workflows/ci.yaml#L653-L680. However, this CI job always runs ty on the same 1,000 fuzzer seeds each time. It compares the PR branch against `main`, and checks that ty does not introduce any _new_ panics on the first 1,000 seeds compared to the `main` branch.

Ideally ty would be sufficiently panic-free (and stack-overflow-free) that we would be able to add a daily CI job invoking ty on _randomized_ fuzzer seeds. We already have such a CI job for the parser -- the CI job runs the parser on 1,000 randomly selected fuzzer seeds every night, and opens an issue if it found any panics: https://github.com/astral-sh/ruff/blob/main/.github/workflows/daily_fuzz.yaml.

This issue tracks adding a similar daily CI job for ty.

---

_Label `ci` added by @AlexWaygood on 2025-12-07 15:39_

---

_Label `testing` added by @AlexWaygood on 2025-12-07 15:39_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-07 15:39_

---

_Comment by @AlexWaygood on 2025-12-07 15:40_

There are currently only two stack overflows (and no panics) on the first 10,000 py-fuzzer seeds, so I'd say we're pretty close to being able to add such a CI job.

---

_Added to milestone `Stable` by @carljm on 2025-12-08 17:44_

---
