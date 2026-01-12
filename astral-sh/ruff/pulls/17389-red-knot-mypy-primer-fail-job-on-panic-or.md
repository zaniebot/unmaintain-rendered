```yaml
number: 17389
title: "[red-knot] mypy_primer: Fail job on panic or internal errors"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/mypy_primer-fail-job-on-panics
created_at: 2025-04-14T11:47:12Z
updated_at: 2025-04-14T11:56:41Z
url: https://github.com/astral-sh/ruff/pull/17389
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] mypy_primer: Fail job on panic or internal errors

---

_@sharkdp_

## Summary

Let the mypy_primer job fail if Red Knot panics or exits with code 2 (indicating an internal error).

Corresponding mypy_primer commit: https://github.com/astral-sh/mypy_primer/commit/90808f4656fa45106b65ad80afe70db0713e26b7

In addition, we may also want to make a successful mypy_primer run required for merging?

## Test Plan

Made sure that mypy_primer exits with code 70 locally on panics, which should result in a pipeline failure, since we only allow code 0 and 1 in the pipeline here: https://github.com/astral-sh/ruff/blob/a4d7c6669bb356eb40e6e7069b11dffbfc27f483/.github/workflows/mypy_primer.yaml#L73

Test PR: https://github.com/astral-sh/ruff/pull/17389

---

_Label `ci` added by @sharkdp on 2025-04-14 11:47_

---

_Label `red-knot` added by @sharkdp on 2025-04-14 11:47_

---

_Comment by @github-actions[bot] on 2025-04-14 11:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @sharkdp on 2025-04-14 11:50_

---

_Closed by @sharkdp on 2025-04-14 11:50_

---

_Branch deleted on 2025-04-14 11:50_

---
