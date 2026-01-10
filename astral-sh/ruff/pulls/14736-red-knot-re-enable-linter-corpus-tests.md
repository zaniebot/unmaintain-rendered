```yaml
number: 14736
title: "[red-knot] Re-enable linter corpus tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/reenable-linter-corpus-tests
created_at: 2024-12-02T15:55:24Z
updated_at: 2024-12-02T19:11:32Z
url: https://github.com/astral-sh/ruff/pull/14736
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Re-enable linter corpus tests

---

_Pull request opened by @sharkdp on 2024-12-02 15:55_

## Summary

Given that no new failures appeared over the past two weeks, and seeing the fuzzing results from @dhruvmanila in #13778, I think we can re-enable these tests. We also had one regression that would have been caught by these tests, so there is some value in having them enabled.



---

_Label `red-knot` added by @sharkdp on 2024-12-02 15:55_

---

_Review requested from @carljm by @sharkdp on 2024-12-02 15:55_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-02 15:55_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-02 15:55_

---

_@AlexWaygood approved on 2024-12-02 15:56_

---

_@carljm approved on 2024-12-02 18:54_

Looks fine to me in principle. Appears there is at least one file that needs adding to `KNOWN_FAILURES` before this can land?

---

_Comment by @github-actions[bot] on 2024-12-02 19:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2024-12-02 19:10_

> Given that no new failures appeared over the past two weeks,

> Appears there is at least one file that needs adding to `KNOWN_FAILURES` before this can land

Fantastic, unfortunate timing on my end :smile:. I accidentally ran `cargo nextest run -p red_knot_python_semantic` offline, instead of `-p red_knot_workspace`.

This new file also contains a self-referential type alias, so this is a known problem (#14672). I added it to the corresponding section in `KNOWN_FAILURES`. 
```py
type K = 'K | None'
```

---

_Merged by @sharkdp on 2024-12-02 19:11_

---

_Closed by @sharkdp on 2024-12-02 19:11_

---

_Branch deleted on 2024-12-02 19:11_

---
