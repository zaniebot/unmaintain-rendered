---
number: 1289
title: noqa for a portion of a file for a specific error
type: issue
state: closed
author: tekumara
labels:
  - suppression
assignees: []
created_at: 2022-12-19T10:09:48Z
updated_at: 2023-05-13T13:05:45Z
url: https://github.com/astral-sh/ruff/issues/1289
synced_at: 2026-01-10T01:22:39Z
---

# noqa for a portion of a file for a specific error

---

_Issue opened by @tekumara on 2022-12-19 10:09_

I have a section of my file with lots of long lines, I'd like to be able to do this:


```
# noqa 501: on
long line 1 ....
long line 2 ....
long line 3 ....
...
# noqa 501: off
```

ie: something [similar to black's `nofmt`](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#code-style:~:text=blocks%20that%20start%20with%20%23%20fmt%3A%20off%20and%20end%20with%20%23%20fmt%3A%20on.%20%23%20fmt%3A%20on/off%20must%20be%20on%20the%20same%20level%20of%20indentation%20and%20in%20the%20same%20block%2C%20meaning%20no%20unindents%20beyond%20the%20initial%20indentation%20level%20between%20them.):

> Black reformats entire files in place. It doesn’t reformat lines that end with # fmt: skip or blocks that start with # fmt: off and end with # fmt: on. # fmt: on/off must be on the same level of indentation and in the same block, meaning no unindents beyond the initial indentation level between

---

_Comment by @charliermarsh on 2022-12-19 21:40_

Yeah I'd like to support this! I'm going to close in favor of #1054.

---

_Closed by @charliermarsh on 2022-12-19 21:40_

---

_Comment by @tekumara on 2023-03-13 02:21_

#1054 is now closed since file-scoped noqa directives have been implemented.

Could we re-open this to support line-based directives for a portion of the file? 🙏 

---

_Reopened by @charliermarsh on 2023-03-13 02:22_

---

_Comment by @charliermarsh on 2023-03-13 02:22_

Yeah that makes sense to me. I don't know what the syntax will be, but I think it makes sense to support this.

---

_Label `noqa` added by @charliermarsh on 2023-03-13 02:22_

---

_Referenced in [astral-sh/ruff#3711](../../astral-sh/ruff/issues/3711.md) on 2023-05-13 11:48_

---

_Comment by @charliermarsh on 2023-05-13 13:05_

Closing in favor of #3711 -- I know this issue came first and was the originating issue, but that one has accumulated more discussion, so seems easier to preserve it.

---

_Closed by @charliermarsh on 2023-05-13 13:05_

---
