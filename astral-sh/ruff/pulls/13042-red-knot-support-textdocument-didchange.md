```yaml
number: 13042
title: "[red-knot] Support `textDocument/didChange` notification"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/did-change-text-document
created_at: 2024-08-22T06:57:35Z
updated_at: 2024-08-23T07:08:09Z
url: https://github.com/astral-sh/ruff/pull/13042
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Support `textDocument/didChange` notification

---

_Pull request opened by @dhruvmanila on 2024-08-22 06:57_

## Summary

This PR adds support for `textDocument/didChange` notification.

There seems to be a bug (probably in Salsa) where it panics with:
```
2024-08-22 15:33:38.802 [info] panicked at /Users/dhruv/.cargo/git/checkouts/salsa-61760caba2b17ca5/f608ff8/src/tracked_struct.rs:377:9:
two concurrent writers to Id(4800), should not be possible
```

## Test Plan

https://github.com/user-attachments/assets/81055feb-ba8e-4acf-ad2f-94084a3efead




---

_Label `red-knot` added by @dhruvmanila on 2024-08-22 06:57_

---

_Comment by @codspeed-hq[bot] on 2024-08-22 07:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/did-change-text-document)

### Merging #13042 will **not alter performance**

<sub>Comparing <code>dhruv/did-change-text-document</code> (28e80b4) with <code>main</code> (c73a7bb)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-22 10:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-08-22 11:24_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-22 11:24_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-22 11:24_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-22 11:24_

---

_@MichaReiser approved on 2024-08-22 11:40_

Nice! Do you also plan on changing `didOpen` and `didClose` to use `apply_changes`?

---

_Comment by @dhruvmanila on 2024-08-22 11:46_

> Nice! Do you also plan on changing `didOpen` and `didClose` to use `apply_changes`?

Yeah, that's done in the previous PR: https://github.com/astral-sh/ruff/pull/13041

---

_@carljm approved on 2024-08-22 19:00_

Sweet!

---

_Merged by @dhruvmanila on 2024-08-23 06:58_

---

_Closed by @dhruvmanila on 2024-08-23 06:58_

---

_Branch deleted on 2024-08-23 06:58_

---
