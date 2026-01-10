```yaml
number: 22319
title: Bump docstring-adder pin
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: AlexWaygood-patch-1
created_at: 2025-12-31T22:33:56Z
updated_at: 2025-12-31T22:37:54Z
url: https://github.com/astral-sh/ruff/pull/22319
synced_at: 2026-01-10T16:36:19Z
```

# Bump docstring-adder pin

---

_Pull request opened by @AlexWaygood on 2025-12-31 22:33_

## Summary

Pulls in https://github.com/astral-sh/docstring-adder/commit/938940acf99b4374c77863df705a40eb45808f1b and https://github.com/astral-sh/docstring-adder/commit/c2ea1baeab2a1696ee67c4b1f59926361f69a087, which mean:
- We get attribute docstrings for non-global-scope attributes
- Improved handling of `sys.version_info` conditions for attribute docstrings
- Improved formatting for attribute docstrings when they appear as the last statement in an `if` suite`, `elif` suite or `else` suite

## Test Plan

The scheduled typeshed-sync worfklow will run in a couple of hours and we'll be able to see if this wrecked anything by eyeballing the changes the bot makes in that PR


---

_Label `ci` added by @AlexWaygood on 2025-12-31 22:33_

---

_Label `ty` added by @AlexWaygood on 2025-12-31 22:33_

---

_Merged by @AlexWaygood on 2025-12-31 22:37_

---

_Closed by @AlexWaygood on 2025-12-31 22:37_

---

_Branch deleted on 2025-12-31 22:37_

---
