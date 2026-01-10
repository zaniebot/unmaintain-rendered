```yaml
number: 8357
title: uv pip show missing --files option
type: issue
state: closed
author: CharlesPerrotMinot
labels:
  - help wanted
  - compatibility
  - cli
  - uv pip
assignees: []
created_at: 2024-10-19T09:54:46Z
updated_at: 2024-10-20T16:13:43Z
url: https://github.com/astral-sh/uv/issues/8357
synced_at: 2026-01-10T04:45:10Z
```

# uv pip show missing --files option

---

_Issue opened by @CharlesPerrotMinot on 2024-10-19 09:54_

Hi!

In order to package lambdas, we currently use `pip show -f [dependency]`.
https://pip.pypa.io/en/stable/cli/pip_show/

Unfortunately, as far as I can tell, this option, which shows the full list of installed files for the given package, isn't supported with `uv pip show`: https://docs.astral.sh/uv/reference/cli/#uv-pip-show

I'm happy if you have a good workaround!
Otherwise, it would be great for this option to be supported.

We basically zip all those files for each dependencies, and push that zip to use in lambda.
I have written a small code that extracts the location from `uv pip show`, and filters the output of a find command, though some packages come as a file instead of a dict (like `six`), and some other aren't installed under their dependency name (`PyJWT` -> `jwt`, `python-dateutil` -> `dateutil`). So it's not very nice haha!

Thanks!

---

_Label `compatibility` added by @zanieb on 2024-10-19 12:57_

---

_Label `cli` added by @zanieb on 2024-10-19 12:57_

---

_Label `uv pip` added by @zanieb on 2024-10-19 12:57_

---

_Label `help wanted` added by @zanieb on 2024-10-19 12:58_

---

_Closed by @charliermarsh on 2024-10-20 16:13_

---

_Closed by @charliermarsh on 2024-10-20 16:13_

---
