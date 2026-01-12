```yaml
number: 7629
title: Ensure that B006 autofixes are inserted after imports
type: pull_request
state: merged
author: hoxbro
labels:
  - bug
assignees: []
merged: true
base: main
head: fix_b006_import
created_at: 2023-09-24T11:53:16Z
updated_at: 2023-09-27T08:27:28Z
url: https://github.com/astral-sh/ruff/pull/7629
synced_at: 2026-01-12T15:55:24Z
```

# Ensure that B006 autofixes are inserted after imports

---

_@hoxbro_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #7616 by ensuring that [B006](https://docs.astral.sh/ruff/rules/mutable-argument-default/#mutable-argument-default-b006) fixes are inserted after module imports.

I have created a new test file, `B006_5.py`. This is mainly because I have been working on this on and off, and the merge conflicts were easier to handle in a separate file. If needed, I can move it into another file. 
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

`cargo test`
<!-- How was it tested? -->


---

_Comment by @codspeed-hq[bot] on 2023-09-24 12:03_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/Hoxbro:fix_b006_import)

### Merging #7629 will **not alter performance**

<sub>Comparing <code>Hoxbro:fix_b006_import</code> (7d27160) with <code>main</code> (2aef46c)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-09-24 12:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @konstin on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:193 on 2023-09-25 08:40_

Do you also need to handle the `locator.full_line_end(statement.end()) == locator.text_len()` case from above here?

---

_Review comment by @konstin on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B006_5.py`:37 on 2023-09-25 08:43_

please add a test case with a statement after the imports, e.g.
```python
def f(x={}):
    import sys

    print(sys.version, x)
```

---

_@konstin approved on 2023-09-25 08:44_

---

_@hoxbro reviewed on 2023-09-25 08:58_

---

_Review comment by @hoxbro on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B006_5.py`:37 on 2023-09-25 08:58_

Do you want with both docstring and import? This example you shown is already there: 
``` python
def import_module_with_values_wrong(value: dict[str, str] = {}):
    import os

    return 2
```

---

_@hoxbro reviewed on 2023-09-25 08:59_

---

_Review comment by @hoxbro on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:193 on 2023-09-25 08:59_

I will take a look at it later today. 

---

_@konstin reviewed on 2023-09-25 09:38_

---

_Review comment by @konstin on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B006_5.py`:37 on 2023-09-25 09:38_

oh sorry i missed that, please disregard my comment

---

_Review comment by @hoxbro on `crates/ruff_linter/src/rules/flake8_bugbear/snapshots/ruff_linter__rules__flake8_bugbear__tests__B006_B006_5.py.snap`:154 on 2023-09-25 18:43_

This line was added when implementing the `locator.full_line_end(statement.end()) == locator.text_len()` case. 

I can see something similar happens when doing it with docstrings on `0.0.291`.

This will:
``` python
def t(foo=[]):
    """Docstring"""

a = 2
```

will be fixed to
``` python
def t(foo=None):
    """Docstring"""
    if foo is None:
        foo = []

a = 2
```

Whereas 

``` python
def t(foo=[]):
    """Docstring"""
```
will be fixed to
``` python
def t(foo=None):
    """Docstring"""

    if foo is None:
        foo = []
```

---

_@hoxbro reviewed on 2023-09-25 18:43_

---

_@hoxbro reviewed on 2023-09-25 18:57_

---

_Review comment by @hoxbro on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:193 on 2023-09-25 18:57_

This should now be handled.

---

_@charliermarsh reviewed on 2023-09-27 01:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:177 on 2023-09-27 01:11_

@Hoxbro - I collapsed these two cases -- I think semantically they're the same? We want to skip docstrings and imports, and find the first opportunity to insert after them. (If there's no such opportunity, we insert at the start of the function.)

---

_Label `bug` added by @charliermarsh on 2023-09-27 01:13_

---

_@charliermarsh reviewed on 2023-09-27 01:14_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:177 on 2023-09-27 01:14_

Of course, please let me know if I've misunderstood or messed something up -- happy to fix in a follow-up.

---

_Merged by @charliermarsh on 2023-09-27 01:26_

---

_Closed by @charliermarsh on 2023-09-27 01:26_

---

_@hoxbro reviewed on 2023-09-27 08:27_

---

_Review comment by @hoxbro on `crates/ruff_linter/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:177 on 2023-09-27 08:27_

No, that sounds right to me.

---

_Branch deleted on 2023-09-27 08:27_

---
