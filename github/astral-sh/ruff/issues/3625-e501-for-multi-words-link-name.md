---
number: 3625
title: E501 for multi-words link name 
type: issue
state: closed
author: FBruzzesi
labels:
  - bug
assignees: []
created_at: 2023-03-20T13:39:52Z
updated_at: 2023-03-26T18:17:36Z
url: https://github.com/astral-sh/ruff/issues/3625
synced_at: 2026-01-07T13:12:14-06:00
---

# E501 for multi-words link name 

---

_Issue opened by @FBruzzesi on 2023-03-20 13:39_

When adding links to docstring, if the link name has multiple words then `E501 Line too long` is raised, even if the link is in a single line. (Related to #3051) 

- Ruff version 0.0.257
- Command `ruff check <module>`
- Minimal code snippet:
    ```python
    def func():
        """
        [this-is-ok](https://loooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooog.url.com)
        [this is not](https://loooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooooog.url.com)
        """
        ...
    ```

---

_Renamed from "Multi words link name " to "E501 for multi-words link name " by @FBruzzesi on 2023-03-20 14:29_

---

_Comment by @charliermarsh on 2023-03-20 16:33_

FWIW, I think the first case is unrelated to the presence of the link -- instead, it passes because there's no whitespace, so there's no way to split up the line.

We may be able to handle the second case by detecting if the last word on the line appears to be a URL, we don't do any Markdown parsing though so from the linter's perspective that line doesn't "end with" a URL.


---

_Comment by @FBruzzesi on 2023-03-20 17:02_

Thank you for getting back so quickly!
Indeed such case could only happen in the docstring. However it gets caught by the parser. 
A possible fix may be to check for the line to match the regex `"^\[.*\]\(https?://\S+\)"`.


---

_Label `bug` added by @charliermarsh on 2023-03-21 02:28_

---

_Referenced in [astral-sh/ruff#3663](../../astral-sh/ruff/pulls/3663.md) on 2023-03-22 02:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-22 03:34_

---

_Closed by @charliermarsh on 2023-03-26 18:17_

---
