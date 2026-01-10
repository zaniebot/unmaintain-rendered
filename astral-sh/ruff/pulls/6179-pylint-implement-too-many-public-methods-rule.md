```yaml
number: 6179
title: "[pylint] Implement `too-many-public-methods` rule (PLR0904)"
type: pull_request
state: merged
author: jelly
labels:
  - rule
assignees: []
merged: true
base: main
head: pylint-too-many-public-methods
created_at: 2023-07-30T13:57:07Z
updated_at: 2024-10-24T12:09:29Z
url: https://github.com/astral-sh/ruff/pull/6179
synced_at: 2026-01-10T20:59:36Z
```

# [pylint] Implement `too-many-public-methods` rule (PLR0904)

---

_Pull request opened by @jelly on 2023-07-30 13:57_

Implement https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/too-many-public-methods.html

Confusingly the rule page mentions a max of 7 while in practice it is 20. https://github.com/search?q=repo%3Apylint-dev%2Fpylint+max-public-methods&type=code

## Summary

Implement pylint's R0904

## Test Plan

Unit tests.


---

_Comment by @github-actions[bot] on 2023-07-30 14:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @jelly on 2023-07-31 09:14_

`python scripts/generate_mkdocs.py` succeeds locally for me, so I am not sure what's going on in CI.

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:110 on 2023-08-01 10:10_

nit: destructuring in the function header is hard to read
```suggestion
    class_Def: &ast::StmtClassDef,
    max_methods: usize,
) {
    ast::StmtClassDef { name, body, .. } = class_def;
    let methods = body
```

---

_Review comment by @konstin on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:112 on 2023-08-01 10:11_

wouldn't this count all fields? Relatedly, could you add some fields to the test case to make sure they aren't counted?

---

_@konstin reviewed on 2023-08-01 10:12_

---

_@jelly reviewed on 2023-08-01 16:18_

---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:112 on 2023-08-01 16:18_

```
    0       │-too_many_public_methods.py:1:7: PLR0904 Too many public methods (10 > 7)
          0 │+too_many_public_methods.py:1:7: PLR0904 Too many public methods (11 > 7)
    1     1 │   |
    2     2 │ 1 | class Everything:
    3     3 │   |       ^^^^^^^^^^ PLR0904
    4       │-2 |     def __init__(self):
    5       │-3 |         pass
          4 │+2 |     foo = 1
```
Woops, this is indeed not functional.

---

_@jelly reviewed on 2023-08-01 16:21_

---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:112 on 2023-08-01 16:21_

Yeah, woops that was indeed wrong.

---

_Review requested from @charliermarsh by @konstin on 2023-08-03 09:45_

---

_Review requested from @konstin by @jelly on 2023-08-08 18:15_

---

_@konstin approved on 2023-08-11 11:17_

lgtm, needs a review from @charliermarsh 

---

_Comment by @jelly on 2023-08-25 15:27_

@charliermarsh gentle ping.

---

_@charliermarsh reviewed on 2023-08-27 15:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:112 on 2023-08-27 15:17_

Can we use `method_visibility` here from `analyze::visibility`? It takes into account dunders and a few other cases.


---

_@charliermarsh reviewed on 2023-08-27 15:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:108 on 2023-08-27 15:17_

Do you know if this rule should be applied to private classes? What about to classes in private modules? Do you mind checking how Pylint treats those?

---

_@jelly reviewed on 2023-08-28 08:56_

---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:108 on 2023-08-28 08:56_

What would a "private module" be in Python? `_module.py` ? As far as I can see [the rule](https://github.com/pylint-dev/pylint/blob/main/pylint/checkers/design_analysis.py#L473) counts the methods except dunders. Well I assume it also ignores `def _foo()` as that would be `private` by convention in Python.

---

_@jelly reviewed on 2023-08-28 09:11_

---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:112 on 2023-08-28 09:11_

`method_visibility` is not public enough (pub crate) it seems to be used here, should it be? Or should it mimmick what the module has an add a `to_visibility`?

---

_@charliermarsh reviewed on 2023-08-28 17:57_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:112 on 2023-08-28 17:57_

I think it's okay to make it `pub` and use it here.

---

_@charliermarsh reviewed on 2023-08-28 17:58_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:108 on 2023-08-28 17:58_

Yeah, something like `_module.py`, which would be considered private based on pydocstyle's definitions of public vs. private visibility for functions.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-08-29 23:15_

---

_@jelly reviewed on 2023-09-13 08:06_

---

_Review comment by @jelly on `crates/ruff/src/rules/pylint/rules/too_many_public_methods.rs`:112 on 2023-09-13 08:06_

Applied, the changes. Please have a look.

---

_Label `rule` added by @charliermarsh on 2023-09-14 00:35_

---

_Merged by @charliermarsh on 2023-09-14 00:52_

---

_Closed by @charliermarsh on 2023-09-14 00:52_

---

_Branch deleted on 2024-10-24 12:09_

---
