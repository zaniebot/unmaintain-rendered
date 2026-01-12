```yaml
number: 15549
title: "[`flake8-bugbear`] Do not raise error if keyword argument is present and target-python version is less or equals than 3.9 (`B903`)"
type: pull_request
state: merged
author: guillaumeLepape
labels:
  - rule
assignees: []
merged: true
base: main
head: b903_suggestion
created_at: 2025-01-17T11:40:17Z
updated_at: 2025-01-17T11:48:55Z
url: https://github.com/astral-sh/ruff/pull/15549
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-bugbear`] Do not raise error if keyword argument is present and target-python version is less or equals than 3.9 (`B903`)

---

_@guillaumeLepape_

\[`flake8-bugbear`\] Do not raise error if keyword argument is present and target-python version is less or equals than 3.9 (`B903`)

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I have this piece of code in one of my repo

```py
class RuleMetadata:
    def __init__(
        self,
        *,
        function: Union[
            Callable[[Session, UserModel, RuleComputeFinaleFromGroupRank], None],
            Callable[[Session, UserModel, RuleComputePoints], None],
        ],
        attribute: str,
        required_admin: bool = False,
    ) -> None:
        self.function = function
        self.attribute = attribute
        self.required_admin = required_admin
```

When I did this, I started by using dataclass, although I wanted the 3 arguments of the __init__ function to be keywords only. As dataclass does not support kw_only before python 3.10 (reference https://docs.python.org/3/library/dataclasses.html#module-contents), I ended up writing the __init__ function myself.

With the current implementation of B903, this will raise an error. My suggestion would be not to raise error if an keyword only argument and target-version<=3.9.

Related github issue : https://github.com/astral-sh/ruff/issues/15548

## Test Plan

I added a new usecase in `class_as_data_structure.py`, a class with keyword only.
I tested this file with the target-version = latest python and with python3.9.
With the latest python, I'm expecting error and python3.9 no error should be raised.


---

_Renamed from "\[`flake8-bugbear`\] Do not raise error if keyword argument is present and target-python version is less or equals than 3.9 (`B903`)" to "[`flake8-bugbear`] Do not raise error if keyword argument is present and target-python version is less or equals than 3.9 (`B903`)" by @guillaumeLepape on 2025-01-17 11:40_

---

_Comment by @github-actions[bot] on 2025-01-17 11:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/78eadb114bcba8872e99dbd0870f895649e546a1/providers/src/airflow/providers/google/cloud/operators/dataflow.py#L64'>providers/src/airflow/providers/google/cloud/operators/dataflow.py:64:1:</a> B903 Class could be dataclass or namedtuple
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| B903 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @MichaReiser on 2025-01-17 11:46_

---

_@MichaReiser reviewed on 2025-01-17 11:48_

Perfect, and the ecosystem check confirms that this resolves a false positive. Thank you so much!

---

_Merged by @MichaReiser on 2025-01-17 11:48_

---

_Closed by @MichaReiser on 2025-01-17 11:48_

---

_Branch deleted on 2025-01-17 11:48_

---
