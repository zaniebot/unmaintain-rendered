```yaml
number: 18736
title: "Change `NoqaCode` to a single string with an offset"
type: pull_request
state: closed
author: ntBre
labels:
  - internal
assignees: []
draft: true
base: main
head: brent/noqa-code-experiments
created_at: 2025-06-17T20:44:43Z
updated_at: 2025-07-24T13:33:35Z
url: https://github.com/astral-sh/ruff/pull/18736
synced_at: 2026-01-12T15:56:24Z
```

# Change `NoqaCode` to a single string with an offset

---

_@ntBre_

## Summary

This PR explores two potential and closely related `NoqaCode` refactors. The first, in the first two commits, replaces the current `NoqaCode(&'static str, &'static str)` with `NoqaCode(&'static str, usize)`, where the `usize` is the index where the prefix and suffix separate. As detailed in [this comment](https://github.com/astral-sh/ruff/pull/18391#issuecomment-2945941780), it's not really possible to get a nice, single `&'static str`, but we can hack around it with a `LazyLock` to call `format!` in a `static`:

https://github.com/astral-sh/ruff/blob/7ff37f42b065f788ad37aed06497842e36860aab/crates/ruff_macros/src/map_codes.rs#L293-L295

In the third commit, I just tried `NoqaCode(String, usize)`, which means `NoqaCode` can no longer be `Copy`, but is otherwise a bit more natural.

## Test Plan

Codspeed benchmarks on this PR. The `&'static str` version didn't show any difference, but the `String` version shows up to a 3% regression. Still, either of these representations should be a bit better if we end up needing to call `Rule::from_code` since they will avoid having to allocate a new string just to pass it there. A single string should be easier to store on `ruff_db::Diagnostic` too.

https://github.com/astral-sh/ruff/blob/9cb02433d5c17bb2ad70c0bf516d99da9f6cd4ae/crates/ruff_linter/src/registry.rs#L19-L21

---

_Renamed from "Change NoqaCode to a single string with an offset" to "Change `NoqaCode` to a single string with an offset" by @ntBre on 2025-06-17 20:44_

---

_Comment by @github-actions[bot] on 2025-06-17 20:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @ntBre on 2025-06-17 23:21_

---

_Comment by @MichaReiser on 2025-06-18 08:09_

I wonder if this refactor is worth it, considering that it's a slow down. Should we wait until we add `secondary_code: String` to `ruff_db::Diagnostic` and then take this up again?

---

_Closed by @ntBre on 2025-07-24 13:33_

---

_Branch deleted on 2025-07-24 13:33_

---
