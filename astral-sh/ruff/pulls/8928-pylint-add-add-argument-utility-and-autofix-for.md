```yaml
number: 8928
title: "[`pylint`] Add `add_argument` utility and autofix for `PLW1514`"
type: pull_request
state: merged
author: qdegraaf
labels:
  - fixes
assignees: []
merged: true
base: main
head: feat/autofixPLW1514
created_at: 2023-11-30T18:38:59Z
updated_at: 2023-12-01T18:31:06Z
url: https://github.com/astral-sh/ruff/pull/8928
synced_at: 2026-01-10T23:40:55Z
```

# [`pylint`] Add `add_argument` utility and autofix for `PLW1514`

---

_Pull request opened by @qdegraaf on 2023-11-30 18:38_

## Summary

- Adds `add_argument` similar to existing `remove_argument` utility to safely add arguments to functions.
- Adds autofix for `PLW1514` as per specs requested in https://github.com/astral-sh/ruff/issues/8883 as a test 

## Test Plan

Checks on existing fixtures as well as additional test and fixture for Python 3.9 and lower fix 

## Issue Link

Closes: https://github.com/astral-sh/ruff/issues/8883

---

_Renamed from "Add autofix for `PLW1514`" to "[`pylint`] Add autofix for `PLW1514`" by @qdegraaf on 2023-11-30 18:39_

---

_Comment by @github-actions[bot] on 2023-11-30 18:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2023-11-30 19:30_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:89 on 2023-11-30 19:30_

Would this introduce a syntax error if the call contains no arguments? Are you interested in writing a more robust `add_argument` utility, similar to the `remove_argument` that exists today?

---

_@qdegraaf reviewed on 2023-11-30 19:54_

---

_Review comment by @qdegraaf on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:89 on 2023-11-30 19:54_

Yes it would. `open()` and similar calls all have a required first argument AFAIK but it's still a pretty dirty and unsafe fix. 

I'd be interested in adding that utility for sure! Will set this to draft and use the autofix as a test case for the utility if there is no urgency behind it.

---

_Renamed from "[`pylint`] Add autofix for `PLW1514`" to "[`pylint`] Add `add_argument` utility and autofix for `PLW1514`" by @qdegraaf on 2023-11-30 19:54_

---

_Converted to draft by @qdegraaf on 2023-11-30 19:54_

---

_@charliermarsh reviewed on 2023-11-30 20:16_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:89 on 2023-11-30 20:16_

Awesome, thanks @qdegraaf!

---

_Review comment by @qdegraaf on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:89 on 2023-12-01 17:07_

Implemented a coarse first version which only handles keyword arguments, but does check for trailing commas and work with empty calls. It explicitly forbids positional arguments but could still maybe break when adding a first argument to a class instantiation.

I could either use the lexer to check for parentheses and panic if they aren't there and leave the implementation of that to a future PR or implement it in this one. What's your preference? If the latter is preferred, if you happen to know of a rule where something like that happens on which I can test the util in absence of unit test infrastructure let me know.

---

_@qdegraaf reviewed on 2023-12-01 17:07_

---

_Marked ready for review by @qdegraaf on 2023-12-01 17:07_

---

_Review requested from @charliermarsh by @charliermarsh on 2023-12-01 17:19_

---

_Label `autofix` added by @charliermarsh on 2023-12-01 17:19_

---

_@charliermarsh reviewed on 2023-12-01 18:06_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/unspecified_encoding.rs`:89 on 2023-12-01 18:06_

I think it's fine to leave this as function-only for now, so we can assume the parentheses always exist.

---

_Merged by @charliermarsh on 2023-12-01 18:23_

---

_Closed by @charliermarsh on 2023-12-01 18:23_

---
