```yaml
number: 14560
title: "[`ruff`] Mark fixes for `unsorted-dunder-all` and `unsorted-dunder-slots` as unsafe when there are complex comments in the sequence (`RUF022`, `RUF023`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - fixes
assignees: []
merged: true
base: main
head: alex/unsafe-sorting
created_at: 2024-11-23T23:18:22Z
updated_at: 2024-11-24T12:51:20Z
url: https://github.com/astral-sh/ruff/pull/14560
synced_at: 2026-01-10T20:50:57Z
```

# [`ruff`] Mark fixes for `unsorted-dunder-all` and `unsorted-dunder-slots` as unsafe when there are complex comments in the sequence (`RUF022`, `RUF023`)

---

_Pull request opened by @AlexWaygood on 2024-11-23 23:18_

## Summary

Fixes #14552

Mark fixes for `RUF022` as unsafe if there are own-line comments in `__all__`, since these are often used to delimit categories within `__all__`:

```py
__all__ = [
    # Core components
    "TaskManager",
    "TaskExecutor",
    # Models
    "TaskContext",
    "TaskProvider",
    "TaskResult",
    # Utilities
    "execute_concurrent",
]
```

Also mark the fix as unsafe if there are ambiguous cases like the following, where you can't say for sure which item the comment should be attached to when the items are reordered:

```py
__all__ = [
    "a", "c",  # comment1
    "b", "d",  # comment2
]
```

Also apply the same changes to `RUF023` (which sorts `__slots__` rather than `__all__`, and adjust the documentation for both rules accordingly.

## Test Plan

`cargo test -p ruff_linter --lib`.

I went through all the existing fixtures for these two rules. All the examples which I would expect to be marked as unsafe fixes are now marked accordingly. All the ones that I would expect to still be marked as safe are still marked as safe.

We already have very good fixture coverage for these rules, so I didn't add any _new_ fixtures.


---

_Label `fixes` added by @AlexWaygood on 2024-11-23 23:18_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-23 23:18_

---

_Comment by @github-actions[bot] on 2024-11-23 23:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -16 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/interactions/application_command.py#L15'>disnake/interactions/application_command.py:15:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/interactions/application_command.py#L15'>disnake/interactions/application_command.py:15:11:</a> RUF022 `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/18d1b97b79adb27609489bfed1809120c9b51993/airflow/__init__.py#L63'>airflow/__init__.py:63:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/18d1b97b79adb27609489bfed1809120c9b51993/airflow/__init__.py#L63'>airflow/__init__.py:63:11:</a> RUF022 `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/8a485331c9b26d534c66acb456c66c680618e195/stubs/lupa/lupa/__init__.pyi#L3'>stubs/lupa/lupa/__init__.pyi:3:11:</a> RUF022 `__all__` is not sorted
+ <a href='https://github.com/python/typeshed/blob/8a485331c9b26d534c66acb456c66c680618e195/stubs/setuptools/setuptools/_distutils/command/__init__.pyi#L22'>stubs/setuptools/setuptools/_distutils/command/__init__.pyi:22:11:</a> RUF022 `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -0 violations, +0 -12 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/__init__.py#L30'>astropy/cosmology/__init__.py:30:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/__init__.py#L30'>astropy/cosmology/__init__.py:30:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/flrw/__init__.py#L4'>astropy/cosmology/flrw/__init__.py:4:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/flrw/__init__.py#L4'>astropy/cosmology/flrw/__init__.py:4:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/realizations.py#L9'>astropy/cosmology/realizations.py:9:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/realizations.py#L9'>astropy/cosmology/realizations.py:9:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/units.py#L19'>astropy/cosmology/units.py:19:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/units.py#L19'>astropy/cosmology/units.py:19:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/io/fits/hdu/base.py#L32'>astropy/io/fits/hdu/base.py:32:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/io/fits/hdu/base.py#L32'>astropy/io/fits/hdu/base.py:32:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/logger.py#L32'>astropy/logger.py:32:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/logger.py#L32'>astropy/logger.py:32:11:</a> RUF022 `__all__` is not sorted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF022 | 18 | 2 | 0 | 0 | 16 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -16 fixes in 5 projects; 50 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/interactions/application_command.py#L15'>disnake/interactions/application_command.py:15:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/d95db29ef84aae61b208853af954ea1ec08e54e3/disnake/interactions/application_command.py#L15'>disnake/interactions/application_command.py:15:11:</a> RUF022 `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -0 violations, +0 -2 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/18d1b97b79adb27609489bfed1809120c9b51993/airflow/__init__.py#L63'>airflow/__init__.py:63:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/apache/airflow/blob/18d1b97b79adb27609489bfed1809120c9b51993/airflow/__init__.py#L63'>airflow/__init__.py:63:11:</a> RUF022 `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/ee0902a832b7fa3e5821ada176566301791e09ec/pandas/api/typing/__init__.py#L36'>pandas/api/typing/__init__.py:36:11:</a> RUF022 `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/8a485331c9b26d534c66acb456c66c680618e195/stubs/lupa/lupa/__init__.pyi#L3'>stubs/lupa/lupa/__init__.pyi:3:11:</a> RUF022 `__all__` is not sorted
+ <a href='https://github.com/python/typeshed/blob/8a485331c9b26d534c66acb456c66c680618e195/stubs/setuptools/distutils/command/__init__.pyi#L22'>stubs/setuptools/distutils/command/__init__.pyi:22:11:</a> RUF022 `__all__` is not sorted
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -0 violations, +0 -12 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/__init__.py#L30'>astropy/cosmology/__init__.py:30:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/__init__.py#L30'>astropy/cosmology/__init__.py:30:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/flrw/__init__.py#L4'>astropy/cosmology/flrw/__init__.py:4:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/flrw/__init__.py#L4'>astropy/cosmology/flrw/__init__.py:4:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/realizations.py#L9'>astropy/cosmology/realizations.py:9:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/realizations.py#L9'>astropy/cosmology/realizations.py:9:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/units.py#L19'>astropy/cosmology/units.py:19:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/cosmology/units.py#L19'>astropy/cosmology/units.py:19:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/io/fits/hdu/base.py#L32'>astropy/io/fits/hdu/base.py:32:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/io/fits/hdu/base.py#L32'>astropy/io/fits/hdu/base.py:32:11:</a> RUF022 `__all__` is not sorted
- <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/logger.py#L32'>astropy/logger.py:32:11:</a> RUF022 [*] `__all__` is not sorted
+ <a href='https://github.com/astropy/astropy/blob/f618f28da47975072bb4faa47df4d4357eae29a1/astropy/logger.py#L32'>astropy/logger.py:32:11:</a> RUF022 `__all__` is not sorted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF022 | 19 | 3 | 0 | 0 | 16 |

</p>
</details>




---

_Comment by @AlexWaygood on 2024-11-23 23:29_

The ecosystem changes look good to me.

---

_@charliermarsh approved on 2024-11-24 02:28_

---

_Merged by @AlexWaygood on 2024-11-24 12:49_

---

_Closed by @AlexWaygood on 2024-11-24 12:49_

---

_Branch deleted on 2024-11-24 12:49_

---
