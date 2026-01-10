```yaml
number: 8283
title: Add newline after module docstrings in preview style
type: pull_request
state: merged
author: konstin
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: module-level-docstrings-get-an-empty-line
created_at: 2023-10-27T12:58:42Z
updated_at: 2024-09-17T07:20:08Z
url: https://github.com/astral-sh/ruff/pull/8283
synced_at: 2026-01-10T21:30:32Z
```

# Add newline after module docstrings in preview style

---

_Pull request opened by @konstin on 2023-10-27 12:58_

Change
```python
"""Test docstring"""
a = 1
```
to
```python
"""Test docstring"""

a = 1
```
in preview style, but don't touch the docstring otherwise.

Do we want to ask black to also format the content of module level docstrings? Seems inconsistent to me that we change function and class docstring indentation/contents but not module docstrings.

Fixes https://github.com/astral-sh/ruff/issues/7995

---

_Label `formatter` added by @konstin on 2023-10-27 12:58_

---

_Label `preview` added by @konstin on 2023-10-27 12:58_

---

_@konstin reviewed on 2023-10-27 12:59_

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/format@expression__number.py.snap`:34 on 2023-10-27 12:59_

This should also be fixed by #8216

---

_@charliermarsh reviewed on 2023-10-28 01:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@expression__number.py.snap`:34 on 2023-10-28 01:04_

Trying to understand, why / how is this happening?

---

_@charliermarsh reviewed on 2023-10-28 01:10_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@expression__number.py.snap`:34 on 2023-10-28 01:10_

Ah, I see, there's a bug in `try_from_docstring` that's fixed in that PR. I'm just gonna copy that fix in here -- seems odd to commit without it.

---

_@charliermarsh approved on 2023-10-28 01:10_

---

_Merged by @charliermarsh on 2023-10-28 01:16_

---

_Closed by @charliermarsh on 2023-10-28 01:16_

---

_Branch deleted on 2023-10-28 01:16_

---

_Comment by @github-actions[bot] on 2023-10-28 01:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no format changes.



---

_@konstin reviewed on 2023-10-28 05:01_

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/format@expression__number.py.snap`:34 on 2023-10-28 05:01_

my plan was to merge the other one first and then rebase, but this does too

---

_@dhruvmanila reviewed on 2023-10-30 05:15_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/statement/suite.rs`:567 on 2023-10-30 05:15_

I don't think bytes can be used as docstrings, right?

```console
$ cat src/play.py                                           
b"docstring"

$ python
Python 3.11.3 (main, Apr 19 2023, 22:45:48) [Clang 16.0.1 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from src import play
>>> play.__doc__
>>> 

$ cat src/play.py
"docstring"

$ python
Python 3.11.3 (main, Apr 19 2023, 22:45:48) [Clang 16.0.1 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from src import play
>>> play.__doc__
'docstring'
>>> 
```

---

_@konstin reviewed on 2023-10-30 09:09_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/statement/suite.rs`:567 on 2023-10-30 09:09_

Thanks for making double check! https://github.com/psf/black/issues/4002

---

_Comment by @peterjc on 2024-03-04 21:21_

This conflicts with https://github.com/asottile/reorder-python-imports v3.12.0 which removes any blank lines between the module docstring and the import lines.

i.e. ruff format in v0.3.0 has the same issue reported in black as https://github.com/psf/black/issues/4175 which is currently unresolved.

P.S. See also https://github.com/asottile/reorder-python-imports/issues/366 - we can expect no fixes there 😞 

*Update: Consensus seems to be switch to isort instead.*

---

_Comment by @nikoethonai on 2024-06-20 07:44_

is there any config property which disables this behavior?

---

_Comment by @MichaReiser on 2024-06-20 07:58_

> is there any config property which disables this behavior?

No, there's no such configuration option.

---

_Comment by @kevalmorabia97 on 2024-09-12 11:37_

I'd like to have an option to disable this behavior. I didn't have this when using Black 24 with preview features on.

---

_Comment by @harupy on 2024-09-17 06:03_

@MichaReiser It looks like a newline is inserted when preview style is disabled. Is this intended?

https://play.ruff.rs/e23adff0-8541-4eab-957b-282e44aef53e

---

_Comment by @harupy on 2024-09-17 07:20_

@MichaReiser Never mind. Just saw https://github.com/astral-sh/ruff/pull/9639.

---
