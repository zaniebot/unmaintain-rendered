---
number: 1481
title: "`uv pip install` cannot fetch requirement file from remote URL"
type: issue
state: closed
author: kdeldycke
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2024-02-16T12:03:07Z
updated_at: 2024-03-06T04:25:30Z
url: https://github.com/astral-sh/uv/issues/1481
synced_at: 2026-01-07T13:12:16-06:00
---

# `uv pip install` cannot fetch requirement file from remote URL

---

_Issue opened by @kdeldycke on 2024-02-16 12:03_

Trying to install packages from a `requirements.txt` file that is available on a remote URL is not supported:
```shell-session
$ uv pip install --requirement https://raw.githubusercontent.com/kdeldycke/workflows/main/requirements.txt
error: failed to open file `https://raw.githubusercontent.com/kdeldycke/workflows/main/requirements.txt`
  Caused by: No such file or directory (os error 2)
```

The same is working without any issue with `pip`:
```shell-session
$ python -m pip install --requirement https://raw.githubusercontent.com/kdeldycke/workflows/main/requirements.txt
Collecting (...)
(...)
Successfully installed (...)
```

That being said, thanks for the fantastic initiative that is `uv` and good luck with the deluge of issues and reports following the launch! 🤗

---

_Label `compatibility` added by @MichaReiser on 2024-02-16 12:14_

---

_Label `enhancement` added by @charliermarsh on 2024-02-16 14:58_

---

_Comment by @charliermarsh on 2024-02-16 14:58_

👍 Yeah this is something we don't support yet. I honestly didn't even know pip supported it :joy: Shouldn't be too hard to add.

---

_Comment by @ottaviohartman on 2024-02-16 23:34_

Is this something I can pick up as a first issue?

---

_Comment by @zanieb on 2024-02-17 04:17_

@omh1280 feel free to give it a try!

---

_Assigned to @ottaviohartman by @zanieb on 2024-02-17 04:17_

---

_Referenced in [ottaviohartman/uv#1](../../ottaviohartman/uv/pulls/1.md) on 2024-02-17 23:03_

---

_Comment by @ottaviohartman on 2024-02-17 23:09_

Thanks @zanieb !

I opened a draft PR into my fork since it's still WIP. I will keep trying to get it to work, but I would appreciate any help to get me unstuck. I've left some comments with questions on that PR.


---

_Referenced in [astral-sh/uv#1643](../../astral-sh/uv/pulls/1643.md) on 2024-02-18 13:43_

---

_Referenced in [callowayproject/bump-my-version#148](../../callowayproject/bump-my-version/issues/148.md) on 2024-02-27 05:49_

---

_Comment by @charliermarsh on 2024-03-06 04:25_

This just merged and will be supported in the next release: https://github.com/astral-sh/uv/pull/2081

---

_Closed by @charliermarsh on 2024-03-06 04:25_

---
