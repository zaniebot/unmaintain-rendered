---
number: 20921
title: "Statistics output doesn't show `[*]` markers for partially-fixable error types"
type: issue
state: closed
author: tsvikas
labels:
  - cli
assignees: []
created_at: 2025-10-16T14:17:33Z
updated_at: 2025-11-27T17:03:37Z
url: https://github.com/astral-sh/ruff/issues/20921
synced_at: 2026-01-10T01:23:01Z
---

# Statistics output doesn't show `[*]` markers for partially-fixable error types

---

_Issue opened by @tsvikas on 2025-10-16 14:17_

### Description

When using `--statistics`, the summary line correctly reports the number of fixable errors, but individual error type lines don't show the fixable marker (`[*]`) when some instances of that error type are fixable.

### Reproduction

Run the following command on the [karpathy/nanochat](https://github.com/karpathy/nanochat) repository (tested using branch `master`, which was on commit `4346536`):

```
❯ ruff check --select ANN --statistics --unsafe-fixes
326     ANN001  missing-type-function-argument
182     ANN201  missing-return-type-undocumented-public-function
 33     ANN204  missing-return-type-special-method
 22     ANN003  missing-type-kwargs
 15     ANN202  missing-return-type-private-function
 12     ANN002  missing-type-args
  7     ANN206  missing-return-type-class-method
Found 597 errors.
[*] 80 fixable with the --fix option.

❯ ruff check --select ANN --statistics --unsafe-fixes --fix
326     ANN001  missing-type-function-argument
137     ANN201  missing-return-type-undocumented-public-function
 22     ANN003  missing-type-kwargs
 12     ANN002  missing-type-args
  9     ANN202  missing-return-type-private-function
  7     ANN206  missing-return-type-class-method
  4     ANN204  missing-return-type-special-method
Found 597 errors (80 fixed, 517 remaining).
```

### Current behavior

Output shows no marker for which of the errors are fixable :

### Expected behavior

Error type lines should indicate fixability:

- `[*]` when all instances are fixable
- `[~]` (or similar symbol) when only some instances are fixable

Alternatively, a new column with the count of fixable issues.

This would clearly communicate that fixes are available for this error type, while distinguishing partial from full fixability.

### Version

ruff 0.14.0


---

_Renamed from "ruff gives a wrong number for the fixable errors" to "Statistics output doesn't show [*] markers for partially-fixable error types" by @tsvikas on 2025-10-16 14:41_

---

_Renamed from "Statistics output doesn't show [*] markers for partially-fixable error types" to "Statistics output doesn't show `[*]` markers for partially-fixable error types" by @tsvikas on 2025-10-16 14:43_

---

_Comment by @ntBre on 2025-10-16 14:59_

Thanks for the report! If I'm reading this correctly, I'm pretty sure we suppress the symbol if _any_ of the diagnostics isn't fixable, which doesn't seem intentional. We sort by code and then fixability, so I think the `fold` always captures an earlier non-fixable diagnostic, if it's available. I wonder if we could just fold directly into an `ExpandedStatistics`.

https://github.com/astral-sh/ruff/blob/73520e4acd9cb259f41a60b0b6396c66ea32950a/crates/ruff/src/printer.rs#L265-L274

---

_Label `cli` added by @ntBre on 2025-10-16 14:59_

---

_Referenced in [astral-sh/ruff#21513](../../astral-sh/ruff/pulls/21513.md) on 2025-11-18 16:21_

---

_Closed by @MichaReiser on 2025-11-27 17:03_

---
