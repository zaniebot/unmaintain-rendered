---
number: 8395
title: noqa not working
type: issue
state: closed
author: sparisi
labels:
  - question
assignees: []
created_at: 2023-10-31T21:17:40Z
updated_at: 2023-10-31T21:45:34Z
url: https://github.com/astral-sh/ruff/issues/8395
synced_at: 2026-01-10T01:22:48Z
---

# noqa not working

---

_Issue opened by @sparisi on 2023-10-31 21:17_

In one file, I'd like `ruff lint .` not to raise 
```
my_plot.py:8:1: F403 `from src.plot_utils import *` used; unable to detect undefined names
my_plot.py:22:16: F405 `make_subplots` may be undefined, or defined from star imports
```

I tried `# noqa: F403` (and 405) to no avail. Tried at the beginning of the file and next to the import lines. I tried `# ruff: disable=F401` (found it in some other issue). Still nothing. 
I am having the same problem with other rules I'd like to ignore. For instance, I have a file where I'd like to ignore line-width, but `# noqa` is not working. 

I am on Windows, latest ruff version. 

---

_Comment by @charliermarsh on 2023-10-31 21:21_

To ignore a rule across a whole file, try adding `# ruff: noqa: F401` on its own line, e.g., at the top of the file.

The `# noqa: F401` syntax (without the `# ruff` prefix) is used to suppress errors _inline_, so you'd add that at the end of a specific line that contains the `F401` error to suppress it.

---

_Label `question` added by @charliermarsh on 2023-10-31 21:21_

---

_Comment by @sparisi on 2023-10-31 21:44_

Thanks that worked! 

---

_Closed by @sparisi on 2023-10-31 21:44_

---

_Comment by @charliermarsh on 2023-10-31 21:45_

No problem! There are some examples here: https://docs.astral.sh/ruff/linter/#error-suppression

---
