```yaml
number: 11827
title: "`ruff server`: Introduce the `ruff.printDebugInformation` command"
type: pull_request
state: closed
author: snowsignal
labels:
  - server
assignees: []
base: main
head: jane/server/command/debug
created_at: 2024-06-10T19:26:39Z
updated_at: 2024-06-10T21:00:24Z
url: https://github.com/astral-sh/ruff/pull/11827
synced_at: 2026-01-12T15:55:39Z
```

# `ruff server`: Introduce the `ruff.printDebugInformation` command

---

_@snowsignal_

## Summary

Closes #11715.

## Test Plan

(TODO: I need to open the corresponding VS Code PR before this is testable)

In VS Code, running the `Print debug information` command should show something like the following in the Output channel:
<img width="632" alt="Screenshot 2024-06-10 at 12 21 07 PM" src="https://github.com/astral-sh/ruff/assets/19577865/85871162-f31e-4671-aeac-1c4e1e1503ad">



---

_Label `server` added by @snowsignal on 2024-06-10 19:26_

---

_Renamed from "`ruff server`: Introduce `ruff.printDebugInformation` command" to "`ruff server`: Introduce the `ruff.printDebugInformation` command" by @snowsignal on 2024-06-10 19:26_

---

_Comment by @codspeed-hq[bot] on 2024-06-10 19:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/jane/server/command/debug)

### Merging #11827 will **improve performances by 14.2%**

<sub>Comparing <code>jane/server/command/debug</code> (05e8e5e) with <code>main</code> (521a358)</sub>



### Summary

`⚡ 1` improvements
`✅ 29` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `jane/server/command/debug` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 917.3 µs | 803.2 µs | +14.2% |


---

_Comment by @github-actions[bot] on 2024-06-10 19:46_

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

_Comment by @snowsignal on 2024-06-10 20:56_

I accidentally pulled in commits from another branch. Closing this in favor of a fresh branch

---

_Closed by @snowsignal on 2024-06-10 20:56_

---

_Branch deleted on 2024-06-10 21:00_

---
