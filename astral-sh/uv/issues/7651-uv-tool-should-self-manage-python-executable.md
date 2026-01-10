---
number: 7651
title: uv tool should self-manage Python executable dependencies
type: issue
state: closed
author: jayqi
labels:
  - question
assignees: []
created_at: 2024-09-24T00:15:46Z
updated_at: 2024-09-24T20:41:20Z
url: https://github.com/astral-sh/uv/issues/7651
synced_at: 2026-01-10T01:24:17Z
---

# uv tool should self-manage Python executable dependencies

---

_Issue opened by @jayqi on 2024-09-24 00:15_

This is related to #7634 and #1640 but not quite the same thing.

Currently, `uv tool` creates virtual environments that symlink to the absolute path of the active Python executable. This means that there is a dependency on a Python that is installed by some other program, e.g. Homebrew, pyenv, mise, asdf, etc. This is a place for things to break if a user upgrades or uninstalls that particular instance of Python. 

Since uv already supports installing python (with `uv python`), having uv _also_ manage the Python interpreter would be better. That means everything involved in the tool installation would be controlled by uv. 

FWIW, this was always something that I found kludgy about pipx. Pipx installations are never fully independent and isolated because of the dependencies on some other externally-managed Python interpreters. 

---

_Comment by @zanieb on 2024-09-24 00:21_

I think this basically exists, you can use `uv tool install --python-preference only-managed` or change your global preference to `only-managed`. We also already use managed interpreter over the system ones (if the required version is installed). I don't think we'll change the default / make this required, but we will continue to improve the behavior of tool installations when we control the interpreter e.g. https://github.com/astral-sh/uv/issues/7287

---

_Comment by @jayqi on 2024-09-24 01:55_

Ah, thanks for pointing to that option and to the ability to set it as a global preference. 

I think it might be helpful to have something in [Tools](https://docs.astral.sh/uv/concepts/tools/) doc that highlights this option and points users to the [Python versions](https://docs.astral.sh/uv/concepts/python-versions/) for more info. I had looked at migrating my tools from pipx before having looked at migrating my Python installation management, so I hadn't encountered the explanation of these options. 

Now that I'm experimenting with uv-managed Python, I think my remaining UX concerns are covered by #7287. 

---

_Label `question` added by @charliermarsh on 2024-09-24 12:19_

---

_Comment by @jayqi on 2024-09-24 20:41_

I've opened #7673 to add more documentation about Python versions for tools.

Otherwise, I still have a remaining concern that tool environments have leaky isolation even when the Python version is managed by uv. `uv tool` doesn't track this dependency, so removing the Python version with `uv python` would still break the tool environment. However, I think this concern is basically a duplicate of #7287. 

I don't think there's anything else so I think this issue can be closed. 

---

_Closed by @jayqi on 2024-09-24 20:41_

---
