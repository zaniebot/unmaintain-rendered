```yaml
number: 7961
title: "[pylint] Implement misplaced-bare-raise (E0704)"
type: pull_request
state: merged
author: clemux
labels:
  - rule
assignees: []
merged: true
base: main
head: pylint_misplaced_bare_raise
created_at: 2023-10-14T17:46:17Z
updated_at: 2023-10-17T03:14:03Z
url: https://github.com/astral-sh/ruff/pull/7961
synced_at: 2026-01-12T02:32:41Z
```

# [pylint] Implement misplaced-bare-raise (E0704)

---

_Pull request opened by @clemux on 2023-10-14 17:46_

Related issue: #970 
 
## Summary

### What it does
This rule triggers an error when a bare raise statement is not in an except or finally block.
### Why is this bad?
If raise statement is not in an except or finally block, there is no active exception to
re-raise, so it will fail with a `RuntimeError` exception.
### Example
```python
def validate_positive(x):
   if x <= 0:
       raise
```
Use instead:
```python
def validate_positive(x):
   if x <= 0:
       raise ValueError(f"{x} is not positive")
```

## Test Plan

Added unit test and snapshot.
Manually compared ruff and pylint outputs on pylint's tests.

## References

 - [pylint documentation](https://pylint.pycqa.org/en/stable/user_guide/messages/error/misplaced-bare-raise.html)
 - [pylint implementation](https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/exceptions.py#L339)



---

_Comment by @github-actions[bot] on 2023-10-14 18:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-10-14 18:49_

Thanks! Do you mind taking a look at some of the violations flagged in the ecosystem check, and seeing how they compare to pylint's behavior?

---

_Comment by @clemux on 2023-10-14 21:14_

They are the same with pylint. I had to run pylint on each file though, I'm not sure what I'm doing wrong. For example, with pip:

```
(venv) clemux@Nardole:~/dev/pip/src$ pylint --disable=all --enable=misplaced-bare-raise .
(venv) clemux@Nardole:~/dev/pip/src$ pylint --disable=all --enable=misplaced-bare-raise pip/_internal/utils/misc.py
************* Module pip._internal.utils.misc
pip/_internal/utils/misc.py:157:4: E0704: The raise statement is not inside an except clause (misplaced-bare-raise)

------------------------------------------------------------------
Your code has been rated at 9.85/10 (previous run: 9.85/10, +0.00)
```

---

_@clemux reviewed on 2023-10-15 16:16_

---

_Review comment by @clemux on `crates/ruff_linter/src/rules/pylint/rules/misplaced_bare_raise.rs`:58 on 2023-10-15 16:16_

I wanted to use [`ruff_python_ast::helpers::RaiseStatementVisitor`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/src/helpers.rs#L895), but it doesn't store the raise statement itself.

It's only used in two rules, so maybe I can refactor that?

---

_@charliermarsh reviewed on 2023-10-16 19:19_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/misplaced_bare_raise.rs`:58 on 2023-10-16 19:19_

Ah yeah -- can we change `RaiseStatementVisitor` to store `Vec<&'a ast::StmtRaise>`?

---

_@clemux reviewed on 2023-10-16 19:23_

---

_Review comment by @clemux on `crates/ruff_linter/src/rules/pylint/rules/misplaced_bare_raise.rs`:58 on 2023-10-16 19:23_

Shall I do that in a separate PR?

---

_@charliermarsh reviewed on 2023-10-16 19:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/misplaced_bare_raise.rs`:65 on 2023-10-16 19:39_

How does this compare to checking `checker.semantic().in_exception_handler()`?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/misplaced_bare_raise.rs`:76 on 2023-10-16 19:40_

Can you include a code example here in the comments to demonstrate the case that this is handling?

---

_@charliermarsh reviewed on 2023-10-16 19:40_

---

_@charliermarsh reviewed on 2023-10-16 19:40_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/misplaced_bare_raise.rs`:58 on 2023-10-16 19:40_

I would be cool with including it here, but either is fine.

---

_@clemux reviewed on 2023-10-16 20:47_

---

_Review comment by @clemux on `crates/ruff_linter/src/rules/pylint/rules/misplaced_bare_raise.rs`:65 on 2023-10-16 20:47_

Well... see the new diff :)
Thanks!

---

_@clemux reviewed on 2023-10-16 20:48_

---

_Review comment by @clemux on `crates/ruff_linter/src/rules/pylint/rules/misplaced_bare_raise.rs`:58 on 2023-10-16 20:48_

Not needed anymore, so I guess I won't touch `RaiseStatementVisitor`.

---

_@clemux reviewed on 2023-10-16 20:49_

---

_Review comment by @clemux on `crates/ruff_linter/src/rules/pylint/rules/misplaced_bare_raise.rs`:76 on 2023-10-16 20:49_

Not needed anymore!

---

_Review requested from @charliermarsh by @charliermarsh on 2023-10-17 02:09_

---

_Label `rule` added by @charliermarsh on 2023-10-17 02:54_

---

_Merged by @charliermarsh on 2023-10-17 03:07_

---

_Closed by @charliermarsh on 2023-10-17 03:07_

---
