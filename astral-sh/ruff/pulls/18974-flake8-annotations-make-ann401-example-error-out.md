```yaml
number: 18974
title: "[`flake8-annotations`] Make `ANN401` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-3
created_at: 2025-06-27T04:07:45Z
updated_at: 2025-06-27T07:12:07Z
url: https://github.com/astral-sh/ruff/pull/18974
synced_at: 2026-01-12T15:56:29Z
```

# [`flake8-annotations`] Make `ANN401` example error out-of-the-box

---

_@MeGaGiGaGon_

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

This PR makes [any-type (ANN401)](https://docs.astral.sh/ruff/rules/any-type/#any-type-ann401)'s example error out-of-the-box

[Old example](https://play.ruff.rs/a392200b-2c51-4812-9cbd-9cb34595e46d)
```py
def foo(x: Any): ...
```

[New example](https://play.ruff.rs/61773d72-0255-4983-a092-b595fd2438b6)
```py
from typing import Any

def foo(x: Any): ...
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-27 04:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/ede3de0ca0addde195199be332d6e4dd454955ef/tests/integration_tests/model_tests.py#L491'>tests/integration_tests/model_tests.py:491:29:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
+ <a href='https://github.com/apache/superset/blob/ede3de0ca0addde195199be332d6e4dd454955ef/tests/unit_tests/pandas_postprocessing/test_rename.py#L65'>tests/unit_tests/pandas_postprocessing/test_rename.py:65:9:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/ede3de0ca0addde195199be332d6e4dd454955ef/tests/integration_tests/model_tests.py#L491'>tests/integration_tests/model_tests.py:491:29:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
+ <a href='https://github.com/apache/superset/blob/ede3de0ca0addde195199be332d6e4dd454955ef/tests/unit_tests/pandas_postprocessing/test_rename.py#L65'>tests/unit_tests/pandas_postprocessing/test_rename.py:65:9:</a> PD002 `inplace=True` should be avoided; it has inconsistent behavior
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PD002 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_@MichaReiser reviewed on 2025-06-27 07:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:479 on 2025-06-27 07:00_

We should also update the `Use instead` section

---

_Label `documentation` added by @MichaReiser on 2025-06-27 07:04_

---

_Merged by @MichaReiser on 2025-06-27 07:06_

---

_Closed by @MichaReiser on 2025-06-27 07:06_

---

_Branch deleted on 2025-06-27 07:10_

---
