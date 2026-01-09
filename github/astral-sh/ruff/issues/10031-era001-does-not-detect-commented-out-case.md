---
number: 10031
title: "`ERA001` does not detect commented out `case` statements"
type: issue
state: closed
author: dosisod
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2024-02-18T20:09:39Z
updated_at: 2024-02-20T03:56:44Z
url: https://github.com/astral-sh/ruff/issues/10031
synced_at: 2026-01-07T13:12:15-06:00
---

# `ERA001` does not detect commented out `case` statements

---

_Issue opened by @dosisod on 2024-02-18 20:09_

Commented out `case` statements are ignored if they are missing a body, or have a body and fit on one line:

```python
match 1:
    # case 1:          # commented code ignored
    # case 1: print()  # commented code ignored

    case 1:
        pass
        # print()  # commented code found

# if 1: print()  # commented code found

if 1:
    pass
    # print()  # commented code found
```

Running:

```
$ ruff --version
ruff 0.2.2

$ ruff file.py --output-format=concise
file.py:7:9: ERA001 Found commented-out code
file.py:9:1: ERA001 Found commented-out code
file.py:13:5: ERA001 Found commented-out code
```

The first 2 `case` blocks should be counted but aren't. Often times I will duplicate and comment out a `case` statement to figure out why a pattern isn't matching, only to forget to remove the commented out case.

The example with the case/body on one line is less of an issue but still odd, perhaps the `case` statement is preventing the body from being detected.

---

_Comment by @MichaReiser on 2024-02-19 07:59_

Thanks for the report. 

I think we need to extend the `POSITIVE_CASES` to handle `case` 

Playground: https://play.ruff.rs/5a305aa9-6e5c-4fa4-999a-8fc427ab9a23

---

_Label `bug` added by @MichaReiser on 2024-02-19 07:59_

---

_Label `help wanted` added by @MichaReiser on 2024-02-19 07:59_

---

_Label `good first issue` added by @MichaReiser on 2024-02-19 07:59_

---

_Referenced in [astral-sh/ruff#10055](../../astral-sh/ruff/pulls/10055.md) on 2024-02-20 00:28_

---

_Comment by @ottaviohartman on 2024-02-20 00:28_

@MichaReiser opened my first PR for this! Let me know how it looks

---

_Closed by @charliermarsh on 2024-02-20 03:56_

---
