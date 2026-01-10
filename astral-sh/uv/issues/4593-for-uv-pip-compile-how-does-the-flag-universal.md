```yaml
number: 4593
title: "For `uv pip compile` how does the flag `--universal` interact with flags `--python-version` and `--python-platform`?"
type: issue
state: closed
author: notatallshaw
labels: []
assignees: []
created_at: 2024-06-27T18:06:41Z
updated_at: 2024-06-27T18:51:46Z
url: https://github.com/astral-sh/uv/issues/4593
synced_at: 2026-01-10T05:31:37Z
```

# For `uv pip compile` how does the flag `--universal` interact with flags `--python-version` and `--python-platform`?

---

_Issue opened by @notatallshaw on 2024-06-27 18:06_

It looks like if I put in `--universal` it respects `--python-version` and ignores `--python-platform`?

In particular if I use `--universal --python-version 3.9`  on an internal requirement file I get the error:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because the requested Python version (3.9) does not satisfy Python>=3.10 and internal-package==0.0.4 depends on Python>=3.10, we can conclude that internal-package==0.0.4 cannot be used.
      And because only internal-package==0.0.4 is available and you require strike-providers, we can conclude that the requirements are unsatisfiable.
```

But if I use `--universal --python-platform linux` I get lines like:

```
pywin32==306 ; sys_platform == 'win32'  # via docker
```

Some documentation and/or warnings about using incompatible flags would be appreciated

---

_Comment by @charliermarsh on 2024-06-27 18:08_

Yes, `--python-platform` is completely ignored if you pass `--universal`. I can make that clearer.

The intent is that `--python-version` is treated as a lower-bound but it might be a bit wonky right now (https://github.com/astral-sh/uv/issues/4591).


---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-27 18:08_

---

_Closed by @charliermarsh on 2024-06-27 18:51_

---

_Closed by @charliermarsh on 2024-06-27 18:51_

---
