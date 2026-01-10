---
number: 4362
title: Possibility to port darglint to ruff?
type: issue
state: closed
author: jsh9
labels: []
assignees: []
created_at: 2023-05-10T23:16:44Z
updated_at: 2023-05-11T03:03:13Z
url: https://github.com/astral-sh/ruff/issues/4362
synced_at: 2026-01-10T01:22:43Z
---

# Possibility to port darglint to ruff?

---

_Issue opened by @jsh9 on 2023-05-10 23:16_

Hi, would it be possible to port [darglint](https://github.com/terrencepreilly/darglint) to ruff?

Darglint differs from pydocstyle in that it actually check the arguments in the docstring against the ground truth (the arg list in the function signature).

Darglint is effective, but it runs quite slow.

Thank you!

---

_Comment by @charliermarsh on 2023-05-11 02:20_

We plan to do this. We haven't started on it yet but it's within scope. Closing in favor of #458 :)

---

_Closed by @charliermarsh on 2023-05-11 02:20_

---

_Comment by @jsh9 on 2023-05-11 03:03_

Thanks!  Great to hear.

---

_Referenced in [astral-sh/ruff#458](../../astral-sh/ruff/issues/458.md) on 2023-05-16 08:47_

---
