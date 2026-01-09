---
number: 8556
title: COM812 breaks conditionals inside multi-line f-strings
type: issue
state: closed
author: parafoxia
labels:
  - bug
assignees: []
created_at: 2023-11-08T11:44:07Z
updated_at: 2023-11-10T04:19:16Z
url: https://github.com/astral-sh/ruff/issues/8556
synced_at: 2026-01-07T13:12:15-06:00
---

# COM812 breaks conditionals inside multi-line f-strings

---

_Issue opened by @parafoxia on 2023-11-08 11:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Synopsis

The COM812 rule is capable of breaking conditional statements within triple f-strings by converting the else-statement into a tuple (or a tuple containing the original tuple).

Of course you can use `# noqa: COM812`, though I think that linters should be incapable of breaking code.

## MRE

(This example assumes the line should be split across multiple lines.)

Original mre.py:
```py
x = f"""This is a test. {
    "Another sentence."
    if True else
    "Alternative route!"
}"""

print(x)
```
```bash
$ py test.py
This is a test. Another sentence.

$ ruff check mre.py
mre.py:4:25: COM812 [*] Trailing comma missing
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

"Fixed" mre.py
```py
x = f"""This is a test. {
    "Another sentence."
    if True else
    "Alternative route!",
}"""

print(x)
```
```bash
$ py test.py
This is a test. ('Another sentence.',)

$ ruff check mre.py
(Exit code 0)
```

## Environment
* Ruff version: 0.1.3
* Python version: 3.9.11
* OS: macOS Sonoma 14.0

---

_Referenced in [astral-sh/ruff#8535](../../astral-sh/ruff/issues/8535.md) on 2023-11-08 11:54_

---

_Label `bug` added by @charliermarsh on 2023-11-09 02:13_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-09 02:13_

---

_Comment by @charliermarsh on 2023-11-09 02:13_

Thanks, I'll take a look at this!

---

_Comment by @dhruvmanila on 2023-11-09 03:13_

I think it must be considering the curly braces as `ContextType::Dict`. It needs to take into account the new f-string tokens and skip if inside a f-string.

---

_Comment by @charliermarsh on 2023-11-09 03:17_

@dhruvmanila - Feel free to fix if you're there, I won't get to it for a few hours.

---

_Comment by @charliermarsh on 2023-11-09 04:01_

Taking a look now.

---

_Referenced in [astral-sh/ruff#8574](../../astral-sh/ruff/pulls/8574.md) on 2023-11-09 04:11_

---

_Closed by @charliermarsh on 2023-11-09 04:25_

---

_Reopened by @dhruvmanila on 2023-11-09 05:03_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-11-09 05:03_

---

_Unassigned @charliermarsh by @dhruvmanila on 2023-11-09 05:03_

---

_Referenced in [astral-sh/ruff#8582](../../astral-sh/ruff/pulls/8582.md) on 2023-11-09 11:44_

---

_Closed by @dhruvmanila on 2023-11-10 04:19_

---
