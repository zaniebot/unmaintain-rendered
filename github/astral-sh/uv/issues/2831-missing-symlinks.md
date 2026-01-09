---
number: 2831
title: missing --symlinks
type: issue
state: open
author: axelande
labels:
  - compatibility
assignees: []
created_at: 2024-04-05T12:11:15Z
updated_at: 2024-06-25T12:22:27Z
url: https://github.com/astral-sh/uv/issues/2831
synced_at: 2026-01-07T13:12:17-06:00
---

# missing --symlinks

---

_Issue opened by @axelande on 2024-04-05 12:11_

I'm trying to debug some native c++ that is a part of my python project with Visual Studio. To get this working with a "pip generated" venv (after ~python 3.10) you need to add `--symlinks` when you create the the `venv` on a Windows machine. Do you think that it would be possible to add this option to uv as well? (So that the files gets created as symlinks instead as of copies..)


---

_Label `compatibility` added by @charliermarsh on 2024-04-05 17:06_

---

_Comment by @charliermarsh on 2024-04-05 17:06_

Do you know why this is necessary for your use-case?

---

_Comment by @axelande on 2024-04-05 17:37_

I'm using it to be able to debugg a c++ extension that starts from python. (I'm running the debugging in visual studio)

---

_Comment by @ncoghlan on 2024-06-25 11:58_

For my current potential `uv` use case, I pass `--symlinks` or `--copies` to `python -m venv` explicitly to guard against the platform-specific defaults ever implicitly changing on me (the virtual environments I'm creating need to do the right thing when packaged up as tarballs and/or zip archives, so I really need to know exactly how the relationships between virtual environments and their base environments are being managed).

Given https://github.com/astral-sh/uv/issues/1795 though, I think I'll back off from trying to replace `python -m venv` for now, and stick with just replacing `pip install`, `pip-compile`, and `pip-sync`.

---

_Comment by @charliermarsh on 2024-06-25 12:03_

@ncoghlan -- Sorry to hijack, but do you have an opinion on the correctness of https://github.com/astral-sh/uv/issues/1795?

---

_Comment by @ncoghlan on 2024-06-25 12:22_

@charliermarsh I replied over there, but I'm pretty sure `python -m venv` is in the wrong in the way it is handling attempts to create virtual environments from within virtual environments (I had to change my current project to explicitly create layered venvs from the base Python runtime, since attempting to create them directly from the next layer down gave an unusable environment)

---
