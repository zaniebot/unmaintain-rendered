```yaml
number: 17315
title: "[red-knot] Add more tests for `*` imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/more-star-import-tests
created_at: 2025-04-09T14:02:43Z
updated_at: 2025-04-09T14:10:32Z
url: https://github.com/astral-sh/ruff/pull/17315
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Add more tests for `*` imports

---

_Pull request opened by @AlexWaygood on 2025-04-09 14:02_

## Summary

- Add more tests for various edge cases, especially around visibility constraints
- Split some tests up and add some more narrative description
- Fix some minor bugs in various tests that weren't actually testing what they were meant to be testing
- Where there is only one module exporting things, and only one module importing things, consistently name the first of these `exporter.py` and the second of these `importer.py`.

## Test Plan

- `uvx pre-commit run -a`
- `cargo test -p red_knot_python_semantic`
- Re-read it several times to make sure it made sense


---

_Label `testing` added by @AlexWaygood on 2025-04-09 14:02_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-09 14:02_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-09 14:02_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-09 14:02_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-09 14:02_

---

_Comment by @github-actions[bot] on 2025-04-09 14:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-04-09 14:10_

This is a somewhat large PR, but I'm going to "bias towards action" and merge without review: I want to rebase https://github.com/astral-sh/ruff/pull/17286 on top of it, land _that_ PR and then move onto other tasks. This PR also only makes changes to our test suite, not to any of our functionality. If anybody has any comments around any of the changes here, I'm very happy to tackle them in followup PRs!

Tests don't seem to be running in CI here... but I ran the tests locally so I'm pretty confident this won't break `main` 🤞

---

_Merged by @AlexWaygood on 2025-04-09 14:10_

---

_Closed by @AlexWaygood on 2025-04-09 14:10_

---

_Branch deleted on 2025-04-09 14:10_

---
