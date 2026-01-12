```yaml
number: 15823
title: "[red-knot] Add version command"
type: pull_request
state: merged
author: tjkuson
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-version
created_at: 2025-01-30T02:49:42Z
updated_at: 2025-02-02T19:16:11Z
url: https://github.com/astral-sh/ruff/pull/15823
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Add version command

---

_@tjkuson_

## Summary

Added version command. Per the linked issue, it's essentially what Ruff does.

Closes  #15497

## Test Plan

```console
$ cargo run -p red_knot -- version
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
     Running `target/debug/red_knot version`
red knot 0.0.0 (58b827fa3 2025-01-30)
```


---

_Comment by @github-actions[bot] on 2025-01-30 03:26_

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

_Marked ready for review by @tjkuson on 2025-01-30 03:38_

---

_Review requested from @carljm by @tjkuson on 2025-01-30 03:38_

---

_Review requested from @MichaReiser by @tjkuson on 2025-01-30 03:38_

---

_Review requested from @AlexWaygood by @tjkuson on 2025-01-30 03:38_

---

_Review requested from @sharkdp by @tjkuson on 2025-01-30 03:38_

---

_Comment by @carljm on 2025-01-30 04:50_

Thanks for the PR! Just FYI it may not be reviewed until next week; @MichaReiser is the brains behind this part of red-knot, and he is out this week.

---

_Label `red-knot` added by @AlexWaygood on 2025-01-30 08:37_

---

_Comment by @MichaReiser on 2025-02-02 18:38_

Thanks, this is great. I did remove the `serialize` implementation because it is unclear to me what it is used for and I changed the snapshot tests to use inline snapshots that are easier to review.

---

_@MichaReiser approved on 2025-02-02 18:39_

---

_Merged by @MichaReiser on 2025-02-02 18:56_

---

_Closed by @MichaReiser on 2025-02-02 18:56_

---

_Branch deleted on 2025-02-02 19:16_

---
