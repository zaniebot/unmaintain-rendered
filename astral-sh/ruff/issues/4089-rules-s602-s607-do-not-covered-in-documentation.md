```yaml
number: 4089
title: Rules S602 - S607 do not covered in documentation
type: issue
state: closed
author: Czaki
labels:
  - bug
  - documentation
assignees: []
created_at: 2023-04-25T07:48:35Z
updated_at: 2023-04-25T17:30:39Z
url: https://github.com/astral-sh/ruff/issues/4089
synced_at: 2026-01-10T11:09:47Z
```

# Rules S602 - S607 do not covered in documentation

---

_Issue opened by @Czaki on 2023-04-25 07:48_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The set of rules from  #3924 was not added to the documentation 

![obraz](https://user-images.githubusercontent.com/3826210/234209438-02e1034c-6638-423a-9ca5-c3ea28690dbc.png)



---

_Comment by @Czaki on 2023-04-25 08:10_

When I build docs locally, then all rules are described. So it looks like a bug in the docs deployment process. 

---

_Comment by @charliermarsh on 2023-04-25 15:03_

Ah yeah, the docs failed to build. Will fix today.

In the interim, you can find these relevant docs in Bandit: https://bandit.readthedocs.io/en/latest/plugins/b602_subprocess_popen_with_shell_equals_true.html.

Note that there's some discussion around whether these rules should be tweaked or even removed (they're consistent with Bandit, but they're quite strict): #4054, #4045.

---

_Label `bug` added by @charliermarsh on 2023-04-25 15:03_

---

_Label `documentation` added by @charliermarsh on 2023-04-25 15:03_

---

_Closed by @charliermarsh on 2023-04-25 17:30_

---
