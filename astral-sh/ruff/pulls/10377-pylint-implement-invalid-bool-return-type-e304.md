```yaml
number: 10377
title: "[`pylint`] Implement `invalid-bool-return-type` (`E304`)"
type: pull_request
state: merged
author: hikaru-kajita
labels:
  - rule
assignees: []
merged: true
base: main
head: invalid-bool-return
created_at: 2024-03-13T05:19:12Z
updated_at: 2024-03-14T00:55:26Z
url: https://github.com/astral-sh/ruff/pull/10377
synced_at: 2026-01-12T15:55:32Z
```

# [`pylint`] Implement `invalid-bool-return-type` (`E304`)

---

_@hikaru-kajita_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `E304` in the issue #970.
Link is here: https://pylint.readthedocs.io/en/stable/user_guide/messages/error/invalid-bool-returned.html
Throws an error when the returning value of `__bool__` method is not boolean

## Test Plan

<!-- How was it tested? -->
I've written it in the `invalid_return_type_bool.py`.



Hi! Actually, this is my first pull request in my life, so please tell me if there is something wrong with the code.
(I used the code written in #4854, and slightly modified it.)
Thanks :)

---

_Comment by @github-actions[bot] on 2024-03-13 05:32_

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

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/invalid_return_type_bool.py`:15 on 2024-03-13 11:54_

Can you add a test case where the variable itself points to a boolean value?

```python
class Bool:
    def __bool__(self):
        x = True
        return x
```

And some more test cases where Ruff _shouldn't_ be raising the violation.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pylint/rules/invalid_bool_return.rs`:16 on 2024-03-13 12:00_

Can you add an `Example` section similar to other docs? Also, a `References` section pointing to the `__bool__` section (https://docs.python.org/3/reference/datamodel.html#object.__bool__).

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/pylint/invalid_return_type_bool.py`:15 on 2024-03-13 12:01_

nit: "fixme" with TODO is redundant

```suggestion
# TODO: Once Ruff has better type checking
```

---

_@dhruvmanila requested changes on 2024-03-13 12:02_

Thanks! And welcome to the project :)

---

_Label `rule` added by @dhruvmanila on 2024-03-13 12:02_

---

_@dhruvmanila approved on 2024-03-13 14:10_

Thank you! If you're interested, I'd love to have a PR which updates the documentation and tests in a similar manner for the `invalid-str-return-type` rule.

---

_Renamed from "implement E304 `invalid-bool-returned` in pylint" to "[`pylint`] Implement `invalid-bool-return-type` (`E304`)" by @dhruvmanila on 2024-03-13 14:11_

---

_Merged by @dhruvmanila on 2024-03-13 14:13_

---

_Closed by @dhruvmanila on 2024-03-13 14:13_

---

_Comment by @hikaru-kajita on 2024-03-13 14:34_

> Thank you! If you're interested, I'd love to have a PR which updates the documentation and tests in a similar manner for the `invalid-str-return-type` rule.

I will work on it! Thank you :)

---

_Branch deleted on 2024-03-14 00:55_

---
