```yaml
number: 1850
title: "Make `uv venv` use the python version listed in `.python-version` if no `--python` is provided"
type: issue
state: closed
author: jfcherng
labels:
  - enhancement
assignees: []
created_at: 2024-02-22T02:53:38Z
updated_at: 2024-08-22T07:19:17Z
url: https://github.com/astral-sh/uv/issues/1850
synced_at: 2026-01-10T04:45:09Z
```

# Make `uv venv` use the python version listed in `.python-version` if no `--python` is provided

---

_Issue opened by @jfcherng on 2024-02-22 02:53_

As the title. People can list multiple versions in the `.python-version` file too. The first matched one should be used.

E.g., with the following `.python-version`, `3.12` is preferred but `3.11` is fine as well.

```
3.12
3.11
```

---

It looks like `uv` project uses [`.python-versions`](https://github.com/astral-sh/uv/blob/21577ad0022027453f120da838fe858ba1fc9b7d/.python-versions) (there is a trailing `s` somehow). `pyenv` uses `.python-version`.

---

_Comment by @zanieb on 2024-02-22 04:22_

HI! That file is for internal development purposes – we need multiple specific Python versions to test this project.

We don't support `.python-version` markers at all. We definitely can consider it though.

---

_Label `enhancement` added by @zanieb on 2024-02-22 04:22_

---

_Comment by @liiight on 2024-03-04 09:15_

+1 for this.
I have a use case where different branches are currently using different python version as part of a transition so using a versioned file to specify this would be great.

Thank you for this wonderful project!

---

_Comment by @Spenhouet on 2024-04-20 07:52_

This becomes especially interesting in combination with https://github.com/astral-sh/uv/issues/2607

---

_Assigned to @zanieb by @zanieb on 2024-06-10 17:54_

---

_Comment by @konstin on 2024-07-01 14:40_

Implemented in https://github.com/astral-sh/uv/pull/4335

---

_Closed by @konstin on 2024-07-01 14:40_

---

_Comment by @jfcherng on 2024-07-01 16:03_

Just leave a note here the command is: `uv venv --preview`

Update: now `--preview` is not needed as of v0.3.0.

---
