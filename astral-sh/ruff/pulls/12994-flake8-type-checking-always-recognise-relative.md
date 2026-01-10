```yaml
number: 12994
title: "`flake8-type-checking`: Always recognise relative imports as first-party"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: alex/cleanup-isort
created_at: 2024-08-19T17:20:16Z
updated_at: 2024-08-19T18:06:59Z
url: https://github.com/astral-sh/ruff/pull/12994
synced_at: 2026-01-10T21:38:32Z
```

# `flake8-type-checking`: Always recognise relative imports as first-party

---

_Pull request opened by @AlexWaygood on 2024-08-19 17:20_

A small refactor. The exact number of dots at the beginning of a relative import isnt relevant to the `isort` classification (and never will be). All that matters is whether there are any dots at the beginning or not. Using a boolean here improves readability and makes it clearer what the argument is for when the function is called from other rules elsewhere in Ruff.

In the process, this fixes what I believe is a small bug in the `flake8-type-checking` code, where we're always passing `level=0` into the `categorize()` function. I think this means we may be miscategorising some imports there which would otherwise be categorised as first-party imports due to them being relative imports.

---

_Label `isort` added by @AlexWaygood on 2024-08-19 17:20_

---

_Comment by @github-actions[bot] on 2024-08-19 17:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @AlexWaygood on 2024-08-19 17:35_

---

_Label `internal` added by @AlexWaygood on 2024-08-19 17:35_

---

_Comment by @MichaReiser on 2024-08-19 17:57_

I guess we can't mark it as internal if it fixes a bug. It's important for users to be aware of this when looking at the changelog

---

_Comment by @AlexWaygood on 2024-08-19 18:00_

> I guess we can't mark it as internal if it fixes a bug. It's important for users to be aware of this when looking at the changelog

It felt unlikely that it would actually come up in practice, and the ecosystem report strengthened my supposition there, so I wondered if it was really worth putting it in the changelog... but definitely don't feel strongly

---

_Label `internal` removed by @AlexWaygood on 2024-08-19 18:00_

---

_Label `isort` removed by @AlexWaygood on 2024-08-19 18:00_

---

_Label `rule` added by @AlexWaygood on 2024-08-19 18:00_

---

_Renamed from "Pass a boolean `is_relative` argument to `isort::categorize`, rather than a u32 `level` argument" to "`flake8-type-checking`: Always recognise relative imports as first-party" by @AlexWaygood on 2024-08-19 18:01_

---

_@MichaReiser approved on 2024-08-19 18:05_

I like it. 

I would be inclined to even use an `enum` with a `Relative` variant to improve readability on the call site but what you have here is good with me too.

---

_Merged by @AlexWaygood on 2024-08-19 18:06_

---

_Closed by @AlexWaygood on 2024-08-19 18:06_

---

_Branch deleted on 2024-08-19 18:06_

---
