```yaml
number: 12498
title: Preserve trailing inline comments on import-from statements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - isort
assignees: []
merged: true
base: main
head: charlie/trailing-comment
created_at: 2024-07-24T22:05:24Z
updated_at: 2024-08-02T15:57:27Z
url: https://github.com/astral-sh/ruff/pull/12498
synced_at: 2026-01-12T15:55:41Z
```

# Preserve trailing inline comments on import-from statements

---

_@charliermarsh_

## Summary

Right now, in the isort comment model, there's nowhere for trailing comments on the _statement_ to go, as in:

```python
from mylib import (
    MyClient,
    MyMgmtClient,
)  # some comment
```

If the comment is on the _alias_, we do preserve it, because we attach it to the alias, as in:

```python
from mylib import (
    MyClient,
    MyMgmtClient,  # some comment
)
```

Similarly, if the comment is trailing on an import statement (non-`from`), we again attach it to the alias, because it can't be parenthesized, as in:

```python
import foo  # some comment
```

This PR adds logic to track and preserve those trailing comments.

We also no longer drop several other comments, like:

```python
from mylib import (
    # some comment
    MyClient
)
```

Closes https://github.com/astral-sh/ruff/issues/12487.


---

_Label `bug` added by @charliermarsh on 2024-07-24 22:05_

---

_Label `isort` added by @charliermarsh on 2024-07-24 22:05_

---

_Comment by @github-actions[bot] on 2024-07-24 22:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+8 -9 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/d6c9941b730855aaa03fd38f0d1216140f90e1ec/pandas/core/generic.py#L3468'>pandas/core/generic.py:3468:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d6c9941b730855aaa03fd38f0d1216140f90e1ec/pandas/core/generic.py#L3629'>pandas/core/generic.py:3629:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d6c9941b730855aaa03fd38f0d1216140f90e1ec/pandas/core/generic.py#L7992'>pandas/core/generic.py:7992:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d6c9941b730855aaa03fd38f0d1216140f90e1ec/pandas/core/generic.py#L7995'>pandas/core/generic.py:7995:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d6c9941b730855aaa03fd38f0d1216140f90e1ec/pandas/core/generic.py#L7998'>pandas/core/generic.py:7998:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d6c9941b730855aaa03fd38f0d1216140f90e1ec/pandas/core/generic.py#L9235'>pandas/core/generic.py:9235:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d6c9941b730855aaa03fd38f0d1216140f90e1ec/pandas/core/generic.py#L9242'>pandas/core/generic.py:9242:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
+ <a href='https://github.com/pandas-dev/pandas/blob/d6c9941b730855aaa03fd38f0d1216140f90e1ec/pandas/core/generic.py#L9245'>pandas/core/generic.py:9245:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -9 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/9eae1ae1a5b47745c7d289bb2092f2b4340e7622/stdlib/pstats.pyi#L17'>stdlib/pstats.pyi:17:13:</a> PYI052 Need type annotation for `CALLS`
- <a href='https://github.com/python/typeshed/blob/9eae1ae1a5b47745c7d289bb2092f2b4340e7622/stdlib/pstats.pyi#L18'>stdlib/pstats.pyi:18:18:</a> PYI052 Need type annotation for `CUMULATIVE`
- <a href='https://github.com/python/typeshed/blob/9eae1ae1a5b47745c7d289bb2092f2b4340e7622/stdlib/pstats.pyi#L19'>stdlib/pstats.pyi:19:16:</a> PYI052 Need type annotation for `FILENAME`
- <a href='https://github.com/python/typeshed/blob/9eae1ae1a5b47745c7d289bb2092f2b4340e7622/stdlib/pstats.pyi#L20'>stdlib/pstats.pyi:20:12:</a> PYI052 Need type annotation for `LINE`
- <a href='https://github.com/python/typeshed/blob/9eae1ae1a5b47745c7d289bb2092f2b4340e7622/stdlib/pstats.pyi#L21'>stdlib/pstats.pyi:21:12:</a> PYI052 Need type annotation for `NAME`
- <a href='https://github.com/python/typeshed/blob/9eae1ae1a5b47745c7d289bb2092f2b4340e7622/stdlib/pstats.pyi#L22'>stdlib/pstats.pyi:22:11:</a> PYI052 Need type annotation for `NFL`
- <a href='https://github.com/python/typeshed/blob/9eae1ae1a5b47745c7d289bb2092f2b4340e7622/stdlib/pstats.pyi#L23'>stdlib/pstats.pyi:23:14:</a> PYI052 Need type annotation for `PCALLS`
- <a href='https://github.com/python/typeshed/blob/9eae1ae1a5b47745c7d289bb2092f2b4340e7622/stdlib/pstats.pyi#L24'>stdlib/pstats.pyi:24:15:</a> PYI052 Need type annotation for `STDNAME`
- <a href='https://github.com/python/typeshed/blob/9eae1ae1a5b47745c7d289bb2092f2b4340e7622/stdlib/pstats.pyi#L25'>stdlib/pstats.pyi:25:12:</a> PYI052 Need type annotation for `TIME`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI052 | 9 | 0 | 9 | 0 | 0 |
| PLW0642 | 8 | 8 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/isort/annotate.rs`:143 on 2024-07-25 06:28_

Shouldn't all remaining comments be guaranteed to start after (or at) `import.end`?

```suggestion
                    while let Some(comment) = comments_iter.next_if(|comment| {
                        comment.start() < import_line_end
```

---

_@MichaReiser approved on 2024-07-25 06:28_

Thanks

---

_@charliermarsh reviewed on 2024-07-25 13:54_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/isort/annotate.rs`:143 on 2024-07-25 13:54_

Good call.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__trailing_comment.py.snap`:58 on 2024-07-25 13:57_

Gonna fix this before merging (the comment should stay on its own line).

---

_@charliermarsh reviewed on 2024-07-25 13:57_

---

_Merged by @charliermarsh on 2024-07-25 21:46_

---

_Closed by @charliermarsh on 2024-07-25 21:46_

---

_Branch deleted on 2024-07-25 21:47_

---
