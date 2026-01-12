```yaml
number: 20448
title: "[`ruff`] Flag fmt: off/on comments around class signatures as invalid (`RUF028`)"
type: pull_request
state: closed
author: danparizher
labels: []
assignees: []
base: main
head: fix-20425
created_at: 2025-09-17T03:27:08Z
updated_at: 2025-10-21T07:26:47Z
url: https://github.com/astral-sh/ruff/pull/20448
synced_at: 2026-01-12T15:57:02Z
```

# [`ruff`] Flag fmt: off/on comments around class signatures as invalid (`RUF028`)

---

_@danparizher_

## Summary

Fixes #20425


---

_Comment by @github-actions[bot] on 2025-09-17 03:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/types/message.py#L108'>disnake/types/message.py:108:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/10a53051e74d2fec2131c4daa6c6a234c98e4dab/scripts/tests/test_validate_docstrings.py#L92'>scripts/tests/test_validate_docstrings.py:92:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
+ <a href='https://github.com/pandas-dev/pandas/blob/10a53051e74d2fec2131c4daa6c6a234c98e4dab/scripts/tests/test_validate_docstrings.py#L9'>scripts/tests/test_validate_docstrings.py:9:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/9913cedb51a39da580d3ef3aff8cff006c3e7fc6/src/_pytest/compat.py#L40'>src/_pytest/compat.py:40:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
+ <a href='https://github.com/pytest-dev/pytest/blob/9913cedb51a39da580d3ef3aff8cff006c3e7fc6/src/_pytest/python.py#L326'>src/_pytest/python.py:326:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
+ <a href='https://github.com/pytest-dev/pytest/blob/9913cedb51a39da580d3ef3aff8cff006c3e7fc6/src/_pytest/python.py#L340'>src/_pytest/python.py:340:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF028 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+6 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/types/message.py#L108'>disnake/types/message.py:108:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/10a53051e74d2fec2131c4daa6c6a234c98e4dab/scripts/tests/test_validate_docstrings.py#L92'>scripts/tests/test_validate_docstrings.py:92:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
+ <a href='https://github.com/pandas-dev/pandas/blob/10a53051e74d2fec2131c4daa6c6a234c98e4dab/scripts/tests/test_validate_docstrings.py#L9'>scripts/tests/test_validate_docstrings.py:9:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pytest-dev/pytest/blob/9913cedb51a39da580d3ef3aff8cff006c3e7fc6/src/_pytest/compat.py#L40'>src/_pytest/compat.py:40:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
+ <a href='https://github.com/pytest-dev/pytest/blob/9913cedb51a39da580d3ef3aff8cff006c3e7fc6/src/_pytest/python.py#L326'>src/_pytest/python.py:326:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
+ <a href='https://github.com/pytest-dev/pytest/blob/9913cedb51a39da580d3ef3aff8cff006c3e7fc6/src/_pytest/python.py#L340'>src/_pytest/python.py:340:1:</a> RUF028 This suppression comment is invalid because it cannot be around a class signature
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF028 | 6 | 6 | 0 | 0 | 0 |

</p>
</details>




---

_Converted to draft by @danparizher on 2025-09-17 13:26_

---

_Marked ready for review by @danparizher on 2025-09-17 22:32_

---

_@MichaReiser requested changes on 2025-09-18 06:56_

Please take a look at the ecosystem checks. I only checked the first and that one looks like a false positive

https://github.com/DisnakeDev/disnake/blob/2e70bb2d617f95a45ab2c31a88b3af2097a767a9/disnake/audit_logs.py#L315

---

_Review requested from @MichaReiser by @danparizher on 2025-09-19 02:40_

---

_Comment by @MichaReiser on 2025-09-19 06:29_

```
+ [disnake/types/message.py:108:1:](https://github.com/DisnakeDev/disnake/blob/7fa65774938b88565c236474973d1ce942081e9e/disnake/types/message.py#L108) RUF028 This suppression comment is invalid because it cannot be around a class signature
```


This one looks incorrect too

---

_Comment by @danparizher on 2025-09-19 20:29_

Might need some help with that one; I tried a few things but can't seem to tie it in to my current implementation. Would appreciate if anyone can take a look!

---

_Comment by @MichaReiser on 2025-10-21 07:26_

Thanks for working on this PR. I'd have to take a close look at the rule myself to suggest hwo to best approach this and I don't have the necessary time right now. That's why I'll close this PR for now but we'd welcome any other PR fixing this issue.

---

_Closed by @MichaReiser on 2025-10-21 07:26_

---
