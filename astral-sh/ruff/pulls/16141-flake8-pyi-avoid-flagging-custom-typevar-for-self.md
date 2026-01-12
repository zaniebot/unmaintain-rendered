```yaml
number: 16141
title: "[flake8-pyi] Avoid flagging `custom-typevar-for-self` on metaclass methods (PYI019)"
type: pull_request
state: merged
author: vladNed
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: bugfix/rule-pyi019-should-not-flag-metaclass-method
created_at: 2025-02-13T17:31:57Z
updated_at: 2025-02-14T05:52:12Z
url: https://github.com/astral-sh/ruff/pull/16141
synced_at: 2026-01-12T15:55:53Z
```

# [flake8-pyi] Avoid flagging `custom-typevar-for-self` on metaclass methods (PYI019)

---

_@vladNed_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Function `custom_type_var_instead_of_self` handling PYI019 was modifying return type to `Self` for metaclasses as the `is_metaclass` check was missing form implementation. 

As discussed in #16127, this is a behavior not accepted by PEP 673

## Test Plan

<!-- How was it tested? -->

- Tested with a metaclass in order to check that the type is not an issue
- Tested with a normal class to check that `Self` is indeed outlined when checking with ruff
- Added a metaclass to resources py files and made a new snapshot

---

_Review requested from @AlexWaygood by @vladNed on 2025-02-13 17:31_

---

_Comment by @github-actions[bot] on 2025-02-13 17:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/rotki/rotki/blob/b0b92e309bb8082871d3af569ae6c6786cafa02a/rotkehlchen/utils/mixins/enums.py#L12'>rotkehlchen/utils/mixins/enums.py:12:16:</a> PYI019 [*] Use `Self` instead of custom TypeVar `T`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI019 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @AlexWaygood on 2025-02-13 18:36_

---

_Label `rule` added by @AlexWaygood on 2025-02-13 18:36_

---

_@AlexWaygood approved on 2025-02-13 18:42_

Thank you! I pushed a very minor simplification, but this is great

---

_Renamed from "[flake8-pyi] Avoid flagging on metaclasses (PYI019)" to "[flake8-pyi] Avoid flagging `custom-typevar-for-self` on metaclass methods (PYI019)" by @AlexWaygood on 2025-02-13 18:42_

---

_Merged by @AlexWaygood on 2025-02-13 18:44_

---

_Closed by @AlexWaygood on 2025-02-13 18:44_

---

_Branch deleted on 2025-02-14 05:52_

---
