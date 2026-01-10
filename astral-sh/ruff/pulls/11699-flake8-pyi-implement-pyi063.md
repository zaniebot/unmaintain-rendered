```yaml
number: 11699
title: "[`flake8-pyi`] Implement `PYI063`"
type: pull_request
state: merged
author: tusharsadhwani
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: pyi-063
created_at: 2024-06-02T23:20:31Z
updated_at: 2024-06-05T23:18:26Z
url: https://github.com/astral-sh/ruff/pull/11699
synced_at: 2026-01-10T21:56:00Z
```

# [`flake8-pyi`] Implement `PYI063`

---

_Pull request opened by @tusharsadhwani on 2024-06-02 23:20_

## Summary
Implements `Y063` from `flake8-pyi`.

## Test Plan
`cargo test` / `cargo insta review`

---

_Review requested from @AlexWaygood by @tusharsadhwani on 2024-06-02 23:20_

---

_@tusharsadhwani reviewed on 2024-06-02 23:22_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/rules/old_style_positional_only_arg.rs`:55 on 2024-06-02 23:22_

This call currently doesn't correctly identify methods, which is why the issue is not correctly raised on line 26 of `PYI063.pyi`. I suspect it is because `scope` is not correct.

I didn't look super hard into it, but I don't see a very straightforward way to get the correct scope from a `FunctionStmt` node.

---

_Comment by @github-actions[bot] on 2024-06-02 23:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+23 -0 violations, +0 -0 fixes in 4 projects; 46 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8b99ad0fbe136371de4c303f7ffee9b5469e77e0/airflow/utils/hashlib_wrapper.py#L28'>airflow/utils/hashlib_wrapper.py:28:9:</a> PYI063 Use PEP 570 syntax for positional-only arguments
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/76c7274985215c487248fa5640e12a9b32a06e8c/pandas/_typing.py#L282'>pandas/_typing.py:282:20:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/pandas-dev/pandas/blob/76c7274985215c487248fa5640e12a9b32a06e8c/pandas/_typing.py#L297'>pandas/_typing.py:297:20:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/pandas-dev/pandas/blob/76c7274985215c487248fa5640e12a9b32a06e8c/pandas/_typing.py#L303'>pandas/_typing.py:303:21:</a> PYI063 Use PEP 570 syntax for positional-only arguments
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+18 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L1072'>src/trio/_socket.py:1072:26:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L1102'>src/trio/_socket.py:1102:21:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L1133'>src/trio/_socket.py:1133:25:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L1151'>src/trio/_socket.py:1151:17:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L1166'>src/trio/_socket.py:1166:26:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L1178'>src/trio/_socket.py:1178:15:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L1183'>src/trio/_socket.py:1183:15:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L1212'>src/trio/_socket.py:1212:13:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L652'>src/trio/_socket.py:652:22:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L662'>src/trio/_socket.py:662:17:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L678'>src/trio/_socket.py:678:13:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L690'>src/trio/_socket.py:690:13:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L696'>src/trio/_socket.py:696:22:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L701'>src/trio/_socket.py:701:15:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L707'>src/trio/_socket.py:707:9:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/_socket.py#L722'>src/trio/_socket.py:722:13:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/testing/_fake_net.py#L469'>src/trio/testing/_fake_net.py:469:15:</a> PYI063 Use PEP 570 syntax for positional-only arguments
+ <a href='https://github.com/python-trio/trio/blob/2741527ef22e98f4d551c2c7c8ebb15628c4e546/src/trio/testing/_fake_net.py#L475'>src/trio/testing/_fake_net.py:475:9:</a> PYI063 Use PEP 570 syntax for positional-only arguments
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/7be95f9b30ffd418483bdb57112f9339465d4695/testing/example_scripts/dataclasses/test_compare_dataclasses_with_custom_eq.py#L11'>testing/example_scripts/dataclasses/test_compare_dataclasses_with_custom_eq.py:11:26:</a> PYI063 Use PEP 570 syntax for positional-only arguments
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI063 | 23 | 23 | 0 | 0 | 0 |

</p>
</details>




---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-03 01:56_

---

_Label `rule` added by @charliermarsh on 2024-06-03 01:56_

---

_Label `preview` added by @charliermarsh on 2024-06-03 01:56_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/definitions.rs`:155 on 2024-06-03 10:36_

We should make sure only to run this rule if the target version is set to Python 3.8 or greater (Ruff still supports Python 3.7), or we'll be recommending invalid syntax for Python 3.7 users

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI063_PYI063.py.snap`:1 on 2024-06-03 10:36_

Any reason not to do this for `.py` files as well? I think type checkers understand the pre-PEP-570 convention for runtime source code as well as stubs

---

_@AlexWaygood reviewed on 2024-06-03 10:37_

Nice! The code here looks really clean. Charlie's assigned this to himself so I'll leave a full review to him, but two quick points:

---

_@VascoSch92 reviewed on 2024-06-03 10:48_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI063.py`:14 on 2024-06-03 10:48_

Can I suggest the test case
```python
def still_fine(_x: int) -> None: ...
```
This should be fine right?

---

_@tusharsadhwani reviewed on 2024-06-03 10:56_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI063_PYI063.py.snap`:1 on 2024-06-03 10:56_

For regular python files, code like:

```python
def add(__a, __b):
    return __a + __b
```

It's not necessary that the user intended to make them positional only. Whereas in stub files it is intended. Raising it on python files can cause false positives.

---

_@AlexWaygood reviewed on 2024-06-03 10:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI063_PYI063.py.snap`:1 on 2024-06-03 10:59_

Although you certainly _can_ do calls such as `add(__a=1, __b=2)`, in my opinion, I think the vast majority of Python functions with parameters like that have probably been written to take advantage of the type-checker convention to understand those parameters as being positional-only. But let's see what Charlie thinks!

---

_@tusharsadhwani reviewed on 2024-06-03 11:07_

---

_Review comment by @tusharsadhwani on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI063.py`:14 on 2024-06-03 11:07_

seems good, I'll add it.

---

_@charliermarsh reviewed on 2024-06-04 02:36_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/old_style_positional_only_arg.rs`:74 on 2024-06-04 02:36_

Why is the check limited to the first two arguments?

---

_@charliermarsh reviewed on 2024-06-04 02:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/old_style_positional_only_arg.rs`:74 on 2024-06-04 02:50_

Ah I see, ok.

---

_@charliermarsh approved on 2024-06-04 02:50_

Thanks!

---

_@charliermarsh reviewed on 2024-06-04 02:50_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI063_PYI063.py.snap`:1 on 2024-06-04 02:50_

I ended up enabling this based on Alex's suggestion.

---

_@charliermarsh reviewed on 2024-06-04 02:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/old_style_positional_only_arg.rs`:55 on 2024-06-04 02:51_

I fixed this by moving it out of the definition phase and into the standard statement checker.

---

_Merged by @charliermarsh on 2024-06-04 03:15_

---

_Closed by @charliermarsh on 2024-06-04 03:15_

---

_Branch deleted on 2024-06-05 23:18_

---
