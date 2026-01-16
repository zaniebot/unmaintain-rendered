```yaml
number: 22616
title: "More fixes to `scripts/conformance.py`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/more-conformance-fixes
created_at: 2026-01-16T11:41:26Z
updated_at: 2026-01-16T12:47:59Z
url: https://github.com/astral-sh/ruff/pull/22616
synced_at: 2026-01-16T12:56:31Z
```

# More fixes to `scripts/conformance.py`

---

_@AlexWaygood_

## Summary

- Escape `|` characters in diagnostic messages, because in the context of a MarkDown table cell `|` can indicate the end of a cell, which is leading to diagnostic messages being truncated in comments. For example, see the "True positives removed" table in https://github.com/astral-sh/ruff/pull/22611#issuecomment-3756748440.
- If the number of total diagnostics increases, format that cell as e.g. `+5` rather than simply `5`.
- Add some missing type annotations and improve one instance of confusing formatting.

## Test Plan

I:
- Built ty on https://github.com/astral-sh/ruff/tree/charlie/functional-dict and saved the binary as `ty-old`
- Built ty on https://github.com/astral-sh/ruff/tree/charlie/functional-typed and save the binary as `ty-new`
- Checked out this PR branch again and ran `uv run --no-project scripts/conformance.py --old-ty=./target/debug/ty-old --new-ty=./target/debug/ty-new --tests-path=../typing/conformance/tests --output=results.md`
- Opened `results.md` in VSCode and verified that the issues detailed in the first two bullets above were fixed when I viewed the rendered MarkDown preview.


---

_Review requested from @MichaReiser by @AlexWaygood on 2026-01-16 11:41_

---

_Label `ci` added by @AlexWaygood on 2026-01-16 11:41_

---

_Label `ty` added by @AlexWaygood on 2026-01-16 11:41_

---

_Comment by @astral-sh-bot[bot] on 2026-01-16 11:42_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-16 11:52_


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

_@MichaReiser approved on 2026-01-16 12:46_

---

_Merged by @AlexWaygood on 2026-01-16 12:47_

---

_Closed by @AlexWaygood on 2026-01-16 12:47_

---

_Branch deleted on 2026-01-16 12:47_

---
