```yaml
number: 15101
title: B901 catches yields in subexprs and more sensitive B901
type: pull_request
state: closed
author: guptaarnav
labels: []
assignees: []
base: main
head: B901-yield-subexpr
created_at: 2024-12-22T20:37:16Z
updated_at: 2024-12-22T21:56:33Z
url: https://github.com/astral-sh/ruff/pull/15101
synced_at: 2026-01-10T20:42:27Z
```

# B901 catches yields in subexprs and more sensitive B901

---

_Pull request opened by @guptaarnav on 2024-12-22 20:37_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Closes #14453 by updating the implementation of the B901 lint rule.  The changes ensure that:
	1.	The visitor now traverses all expressions within statements, ensuring yield expressions embedded are correctly flagged even if they are not the expression of an `ExprStmt`.
	2.     The rule now correctly identifies yield and yield from expressions when they appear as the right-hand side of assignment statements (e.g., x = yield or x = yield from []); this catches the case mentioned in the original issue by @AlexWaygood.
	3.     We still do not recurse into lambda expressions or nested function defs as these are evaluated separately.


## Test Plan
I added the tests mentioned by @MichaReiser and @AlexWaygood in the original issue into `test/fixtures/flake8_bugbear/B901.py` and updated the related snapshot.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-12-22 20:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+32 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/a670d60c61b5d4e2b0506eb8b6c7b053ad26d409/src/trio/_core/_traps.py#L63'>src/trio/_core/_traps.py:63:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+31 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/assertion/__init__.py#L188'>src/_pytest/assertion/__init__.py:188:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/cacheprovider.py#L298'>src/_pytest/cacheprovider.py:298:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/cacheprovider.py#L419'>src/_pytest/cacheprovider.py:419:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/cacheprovider.py#L461'>src/_pytest/cacheprovider.py:461:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/capture.py#L872'>src/_pytest/capture.py:872:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/capture.py#L877'>src/_pytest/capture.py:877:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/capture.py#L882'>src/_pytest/capture.py:882:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/capture.py#L887'>src/_pytest/capture.py:887:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/config/__init__.py#L1435'>src/_pytest/config/__init__.py:1435:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/debugging.py#L303'>src/_pytest/debugging.py:303:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/faulthandler.py#L88'>src/_pytest/faulthandler.py:88:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/helpconfig.py#L133'>src/_pytest/helpconfig.py:133:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/logging.py#L780'>src/_pytest/logging.py:780:17:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/logging.py#L788'>src/_pytest/logging.py:788:17:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/logging.py#L801'>src/_pytest/logging.py:801:17:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/logging.py#L869'>src/_pytest/logging.py:869:17:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/pytester.py#L171'>src/_pytest/pytester.py:171:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/setuponly.py#L36'>src/_pytest/setuponly.py:36:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/skipping.py#L257'>src/_pytest/skipping.py:257:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/skipping.py#L292'>src/_pytest/skipping.py:292:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/terminal.py#L683'>src/_pytest/terminal.py:683:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/terminal.py#L914'>src/_pytest/terminal.py:914:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/terminal.py#L925'>src/_pytest/terminal.py:925:13:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/tmpdir.py#L313'>src/_pytest/tmpdir.py:313:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/unittest.py#L428'>src/_pytest/unittest.py:428:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/warnings.py#L110'>src/_pytest/warnings.py:110:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/warnings.py#L119'>src/_pytest/warnings.py:119:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/warnings.py#L129'>src/_pytest/warnings.py:129:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/warnings.py#L90'>src/_pytest/warnings.py:90:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/src/_pytest/warnings.py#L99'>src/_pytest/warnings.py:99:9:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
+ <a href='https://github.com/pytest-dev/pytest/blob/24e84f08f43216f95620135305cbebe9f646e433/testing/conftest.py#L89'>testing/conftest.py:89:5:</a> B901 Using `yield` and `return {value}` in a generator function can lead to confusing behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B901 | 32 | 32 | 0 | 0 | 0 |

</p>
</details>




---

_Closed by @guptaarnav on 2024-12-22 21:06_

---

_Comment by @guptaarnav on 2024-12-22 21:07_

Saw a large number of violations from the automated ruff-ecosystem check.  primarily seems to be from code that says `return (yield)` which is valid.  rethinking this PR, closing for now.

---
