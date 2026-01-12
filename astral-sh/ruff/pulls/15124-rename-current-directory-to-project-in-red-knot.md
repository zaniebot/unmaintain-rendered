```yaml
number: 15124
title: "Rename `--current-directory` to `--project` in Red Knot benchmark script"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: micha/fix-knot-benchmark
created_at: 2024-12-23T12:45:52Z
updated_at: 2024-12-23T12:50:36Z
url: https://github.com/astral-sh/ruff/pull/15124
synced_at: 2026-01-12T15:55:50Z
```

# Rename `--current-directory` to `--project` in Red Knot benchmark script

---

_@MichaReiser_

## Summary

I renamed `--current-directory` to `--project` to align with uv but forgot to update the red knot benchmark script.
The result was that all knot benchmarks completed in 5ms (instantely)

This PR updates the CLI option. 

## Test Plan

The benchmarks run successfully. 


---

_Review requested from @carljm by @MichaReiser on 2024-12-23 12:45_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-23 12:45_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-23 12:45_

---

_Label `internal` added by @MichaReiser on 2024-12-23 12:45_

---

_Merged by @MichaReiser on 2024-12-23 12:50_

---

_Closed by @MichaReiser on 2024-12-23 12:50_

---

_Branch deleted on 2024-12-23 12:50_

---
