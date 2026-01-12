```yaml
number: 13302
title: "[`pydoclint`] Allow ignoring one line docstrings for `DOC` rules"
type: pull_request
state: merged
author: augustelalande
labels:
  - configuration
  - docstring
assignees: []
merged: true
base: main
head: pydoclint
created_at: 2024-09-10T05:19:58Z
updated_at: 2025-10-25T20:30:06Z
url: https://github.com/astral-sh/ruff/pull/13302
synced_at: 2026-01-12T15:55:43Z
```

# [`pydoclint`] Allow ignoring one line docstrings for `DOC` rules

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add a setting to allow ignoring one line docstrings for the pydoclint rules.

Resolves #13086

Part of #12434

## Test Plan

Run tests with setting enabled.


---

_Renamed from "[`pydoclint`] Allow ignoring one line docstrings in for `DOC` rules" to "[`pydoclint`] Allow ignoring one line docstrings for `DOC` rules" by @augustelalande on 2024-09-10 05:20_

---

_Comment by @github-actions[bot] on 2024-09-10 05:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-09-10 13:42_

See my question in https://github.com/astral-sh/ruff/issues/13086#issuecomment-2340837809 -- I'm not fully sure I understand why this option is desirable yet

---

_Label `configuration` added by @AlexWaygood on 2024-09-10 20:22_

---

_Label `docstring` added by @AlexWaygood on 2024-09-10 20:22_

---

_Comment by @jsh9 on 2025-01-13 08:50_

Hi, over on _pydoclint_, I have a config option that does the same thing, and it has a different name: `--skip-checking-short-docstrings`.  (More details here: https://github.com/astral-sh/ruff/issues/12434#issuecomment-2586035296)

I think `ignore-one-line-docstrings` is also a good name.  If you want to keep using this name, maybe you can add a note in the documentation to say that this `ignore-one-line-docstrings` is the same as `skip-checking-short-docstrings`?

---

_Label `needs-decision` added by @MichaReiser on 2025-01-13 09:30_

---

_Comment by @AlexWaygood on 2025-01-13 12:30_

I'm now persuaded that this configuration option is a good idea -- see my comment at https://github.com/astral-sh/ruff/issues/13086#issuecomment-2586978787

---

_Label `needs-decision` removed by @AlexWaygood on 2025-01-13 12:30_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-13 12:30_

---

_Review request for @MichaReiser removed by @MichaReiser on 2025-01-13 12:41_

---

_Review requested from @dylwil3 by @dylwil3 on 2025-01-14 16:45_

---

_Comment by @dylwil3 on 2025-01-16 22:05_

Thanks for the great contribution!

I decided to leave the name as-is and write a short note in the docs about the corresponding name in `pydoclint` - I like that `ignore-one-line-docstrings` makes it unambiguous what counts as a "short" docstring.

Apologies for the delay in resolving this PR!

---

_Merged by @dylwil3 on 2025-01-16 22:05_

---

_Closed by @dylwil3 on 2025-01-16 22:05_

---

_Comment by @augustelalande on 2025-01-16 22:09_

Thank you @dylwil3 as always

---

_Branch deleted on 2025-10-25 20:30_

---
