---
number: 8020
title: D400 should accept question mark as a sentence ender
type: issue
state: closed
author: warsaw
labels:
  - docstring
assignees: []
created_at: 2023-10-17T16:05:51Z
updated_at: 2023-11-10T18:48:56Z
url: https://github.com/astral-sh/ruff/issues/8020
synced_at: 2026-01-07T13:12:15-06:00
---

# D400 should accept question mark as a sentence ender

---

_Issue opened by @warsaw on 2023-10-17 16:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

`D400` complains when the first line of a docstring ends in a `?`.   However, it seems natural when documenting a predicate function that the first line of the docstring poses a question.  `D400` should accept `?` as a sentence ender in those cases.

```python
# example.py
def is_a_thing(thing: int) -> bool:
    """Is this a thing?"""
    return bool(thing)
```

```toml
# ruff.toml
select = ['D400']
```

```shell
$ ruff check example.py 
example.py:2:5: D400 First line should end with a period
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

---

_Label `formatter` added by @zanieb on 2023-10-17 17:48_

---

_Label `needs-decision` added by @zanieb on 2023-10-17 17:48_

---

_Comment by @charliermarsh on 2023-10-18 22:48_

Following pydocstyle, I think there's a different rule for this (`D415`). Could you disable `D400` and enable `D415` instead?

---

_Comment by @warsaw on 2023-10-18 23:11_

Oh interesting.  I tried to boil it down to an easy reproducer, but actually, in my production code (which I can't share), I have `select = ['D']` rather than specifically `D400`.  Maybe there's a weird interaction going on here?

Yup, if I change the `ruff.toml` to just:

```toml
# ruff.toml
select = ['D']
```

I get

```
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
foo.py:1:1: D100 Missing docstring in public module
foo.py:2:5: D400 [*] First line should end with a period
Found 2 errors.
[*] 1 potentially fixable with the --fix option.
```


---

_Label `formatter` removed by @MichaReiser on 2023-10-18 23:12_

---

_Renamed from "Formatter: D400 should accept question mark as a sentence ender" to "D400 should accept question mark as a sentence ender" by @MichaReiser on 2023-10-18 23:12_

---

_Comment by @charliermarsh on 2023-10-18 23:25_

Do you have a convention set? Like:

```toml
[tool.ruff.pydocstyle]
convention = "google"
```

That effectively acts as a filter on the select.

---

_Comment by @warsaw on 2023-10-18 23:34_

Yes, I'm using `convention = 'pep257'`.

---

_Comment by @charliermarsh on 2023-11-09 05:03_

I think the solution here is to fix #2606, then suggest using `D415` in lieu of `D400`. Right now, it's impossible to enable `D415` alongside that convention.

---

_Comment by @charliermarsh on 2023-11-10 18:48_

Okay, in the next release, you should be able to add:

```toml
select = ["D", "D415"]
ignore = ["D400"]

[pydocstyle]
convention = "pep257"
```

To enable the PEP 257 convention, but swapping out D400 for D415.

---

_Closed by @charliermarsh on 2023-11-10 18:48_

---

_Label `docstring` added by @charliermarsh on 2023-11-10 18:48_

---

_Label `needs-decision` removed by @charliermarsh on 2023-11-10 18:48_

---
