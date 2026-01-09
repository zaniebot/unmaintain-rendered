---
number: 10424
title: ruff format vs ruff check output and verbose option
type: issue
state: closed
author: isparks-tg
labels:
  - question
assignees: []
created_at: 2024-03-15T16:24:59Z
updated_at: 2024-03-15T16:41:46Z
url: https://github.com/astral-sh/ruff/issues/10424
synced_at: 2026-01-07T13:12:15-06:00
---

# ruff format vs ruff check output and verbose option

---

_Issue opened by @isparks-tg on 2024-03-15 16:24_

When running `ruff format --check` you get a count of files unchanged. When running `ruff check` you get nothing returned if there is nothing to report.

```
$ ruff format . --check
2628 files already formatted
$ ruff check .
$ 
```
In our use case we include the output of these commands into some customer deliverables and in the case of `ruff check .` they can't tell the difference between "This does nothing" and "There was nothing to do".

We can run with the verbose option -v but it generates a ton of output way overkill for our needs. 

I note also that the -v flag makes the verbose output go to stderr and not stdout. Is this deliberate?

```
$ ruff check . -v > program.stdout 2> program.stderr
$ wc program.stderr
   8425  108402 2300293 program.stderr
$ wc program.stdout
0 0 0 program.stdout
```

What we would like is symmetry between `ruff format` and `ruff check` so that a count of files checked are reported by `ruff check`

Very grateful for `ruff`, brilliant tool, I hope the above does not come over demanding. Thanks for your consideration. 

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.

verbose, ruff check output

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

[tool.ruff]

target-version = "py311"
line-length = 120
indent-width = 4
lint.extend-select = ["C90", "S", "T", "I", "E", "W", "PIE"]
lint.ignore = ["PIE808", "PIE810"]
lint.extend-ignore = ["S101","S320", "S701"]

* The current Ruff version (`ruff --version`).

ruff 0.3.2


---

_Comment by @zanieb on 2024-03-15 16:29_

Please see #8553 — we will indicate successful checks in the next release. I think including a file count makes sense too it just needs to be implemented.

And yes it is intentional that logs go to standard error since it's not "data" meant to be piped to another tool.

---

_Label `question` added by @zanieb on 2024-03-15 16:30_

---

_Comment by @isparks-tg on 2024-03-15 16:41_

Thanks @zanieb I missed the mention of logging in relation to the -v option because I was looking for (slightly) more verbose output.

Closing, thanks for the tool and the helpful response.

---

_Closed by @isparks-tg on 2024-03-15 16:41_

---

_Referenced in [astral-sh/ruff#20365](../../astral-sh/ruff/issues/20365.md) on 2025-09-12 16:49_

---
