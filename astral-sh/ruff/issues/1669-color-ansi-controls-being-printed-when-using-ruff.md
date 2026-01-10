---
number: 1669
title: color ANSI controls being printed when using ruff on a GitBash console on windows
type: issue
state: closed
author: joaompinto
labels:
  - question
assignees: []
created_at: 2023-01-05T21:07:58Z
updated_at: 2023-01-05T21:54:45Z
url: https://github.com/astral-sh/ruff/issues/1669
synced_at: 2026-01-10T01:22:39Z
---

# color ANSI controls being printed when using ruff on a GitBash console on windows

---

_Issue opened by @joaompinto on 2023-01-05 21:07_

The problem only happens when running GitBash within a vscode terminal window, on a regular GitBash window the ANSI emulation works as expected.

```
$ ruff .
Found 1 error(s).
←[1mtest.py←[0m←[36m:←[0m1←[36m:←[0m8←[36m:←[0m ←[1;31mF401←[0m `httpx` imported but unused
1 potentially fixable with the --fix option.
$ flake8
.\test.py:1:1: F401 'httpx' imported but unused

```

---

_Comment by @joaompinto on 2023-01-05 21:08_

![ruff_controls](https://user-images.githubusercontent.com/1143125/210880221-54196928-027d-4d26-a7f0-37856d992934.PNG)


---

_Comment by @charliermarsh on 2023-01-05 21:52_

There's an environment variable you can set to disable them entirely -- is that sufficient?

E.g. `NO_COLOR=1 ruff .`


---

_Label `question` added by @charliermarsh on 2023-01-05 21:52_

---

_Comment by @joaompinto on 2023-01-05 21:54_

Yes, that works for me. Thanks for the quick reply.

---

_Closed by @joaompinto on 2023-01-05 21:54_

---
