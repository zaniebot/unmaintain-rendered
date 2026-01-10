```yaml
number: 15996
title: "[red-knot] Fixup a couple of nits in the `red_knot_test` README"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/md-nits
created_at: 2025-02-06T15:00:52Z
updated_at: 2025-02-06T15:04:29Z
url: https://github.com/astral-sh/ruff/pull/15996
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Fixup a couple of nits in the `red_knot_test` README

---

_Pull request opened by @AlexWaygood on 2025-02-06 15:00_

## Summary

- Fix another `Literal`/`Literate` typo
- Use a fancy `[!NOTE]` admonition for the note about `dir-test` and `rstest`. GitHub has special rendering for these admonitions (h/t @BurntSushi since I learned about this from https://github.com/astral-sh/ruff/pull/15945!

## Test Plan

- I checked that the "Note" admonition renders well on GitHub: https://github.com/astral-sh/ruff/blob/ac8d51961c893fa99f623f6ecee922f75114e3eb/crates/red_knot_test/README.md
- Ran `uvx pre-commit run -a`


---

_Label `red-knot` added by @AlexWaygood on 2025-02-06 15:00_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-06 15:00_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-06 15:00_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-06 15:00_

---

_@BurntSushi approved on 2025-02-06 15:03_

---

_Merged by @AlexWaygood on 2025-02-06 15:04_

---

_Closed by @AlexWaygood on 2025-02-06 15:04_

---

_Branch deleted on 2025-02-06 15:04_

---
