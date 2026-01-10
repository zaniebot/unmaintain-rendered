```yaml
number: 19097
title: "[`flake8-pyi`] Make example error out-of-the-box (`PYI014`, `PYI015`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-07-02T18:05:20Z
updated_at: 2025-07-03T14:16:41Z
url: https://github.com/astral-sh/ruff/pull/19097
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-pyi`] Make example error out-of-the-box (`PYI014`, `PYI015`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-02 18:05_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

Both in one PR since they are in the same file

No playground links since the playground does not support rules that only apply to PYI files

PYI014
---

This PR makes [argument-default-in-stub (PYI014)](https://docs.astral.sh/ruff/rules/argument-default-in-stub/#argument-default-in-stub-pyi014)'s example error out-of-the-box

Old example:
```
PS ~\Desktop\New_folder\ruff>echo @"
```
```py
def foo(arg=[]) -> None: ...
```
```
"@ | uvx ruff check --isolated --preview --select PYI014 --stdin-filename "test.pyi" -
```
```
All checks passed!
```

New example:
```
PS ~\Desktop\New_folder\ruff>echo @"
```
```py
def foo(arg=bar()) -> None: ...
```
```
"@ | uvx ruff check --isolated --preview --select PYI014 --stdin-filename "test.pyi" -
```
```snap
test.pyi:1:13: PYI014 [*] Only simple default values allowed for arguments
  |
1 | def foo(arg=bar()) -> None: ...
  |             ^^^^^ PYI014
  |
  = help: Replace default value with `...`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

PYI015
---

This PR makes [assignment-default-in-stub (PYI015)](https://docs.astral.sh/ruff/rules/assignment-default-in-stub/#assignment-default-in-stub-pyi015)'s example error out-of-the-box

Old example:
```
PS ~\Desktop\New_folder\ruff>echo @"
```
```py
foo: str = "..."
```
```
"@ | uvx ruff check --isolated --preview --select PYI015 --stdin-filename "test.pyi" -
```
```
All checks passed!
```

New example:
```
PS ~\Desktop\New_folder\ruff>echo @"
```
```py
foo: str = bar()
```
```
"@ | uvx ruff check --isolated --preview --select PYI015 --stdin-filename "test.pyi" -
```
```snap
test.pyi:1:12: PYI015 [*] Only simple default values allowed for assignments
  |
1 | foo: str = bar()
  |            ^^^^^ PYI015
  |
  = help: Replace default value with `...`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Review requested from @AlexWaygood by @MeGaGiGaGon on 2025-07-02 18:05_

---

_Comment by @github-actions[bot] on 2025-07-02 18:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-07-03 11:54_

---

_Merged by @AlexWaygood on 2025-07-03 11:54_

---

_Closed by @AlexWaygood on 2025-07-03 11:54_

---

_Label `documentation` added by @AlexWaygood on 2025-07-03 11:54_

---

_Branch deleted on 2025-07-03 14:16_

---
