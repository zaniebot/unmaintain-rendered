```yaml
number: 17428
title: uv and system installed Python installations
type: issue
state: closed
author: zoof
labels:
  - question
assignees: []
created_at: 2026-01-12T23:07:20Z
updated_at: 2026-01-13T15:13:34Z
url: https://github.com/astral-sh/uv/issues/17428
synced_at: 2026-01-13T15:29:30Z
```

# uv and system installed Python installations

---

_@zoof_

### Question

My organization is considering adopting uv but I have a question.  For security reasons, we'd like to have uv use a version of Python that has been centrally installed so that it is kept up-to-date.  If an environment has been set up to use this Python installation, and Python gets updated by system administrators, say from version 3.12 to 3.13, what are the consequences for existing venv's?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @zoof on 2026-01-12 23:07_

---

_Comment by @zanieb on 2026-01-12 23:11_

When you update Python do you delete the old interpreter / installation?

---

_Comment by @zoof on 2026-01-13 14:24_

I guess my question is what happens if the old one gets deleted -- short term this is not a problem but long term, old versions may be deprecated and may be centrally uninstalled.

And a related question is, if there are two Python versions installed, and UV_NO_MANAGED_PYTHON="true", which version of Python will uv use for new environments?

---

_Comment by @zanieb on 2026-01-13 14:42_

If you delete it, the virtual environment will be broken. In projects, uv will generally transparently replace it with a new one on `sync`.

>  if there are two Python versions installed, and UV_NO_MANAGED_PYTHON="true", which version of Python will uv use for new environments?

We'll use the one that comes first on the `PATH`, unless you've pinned a specific one.

---

_Comment by @zoof on 2026-01-13 14:53_

> If you delete it, the virtual environment will be broken. In projects, uv will generally transparently replace it with a new one on `sync`.

So for a broken environment, a uv sync will fix it?  Say Python is upgraded from 3.12 to 3.13, will the environment continue using 3.12 or does the NO_MANAGED_PYTHON flag force it to upgrade?

> We'll use the one that comes first on the `PATH`, unless you've pinned a specific one.

Ok, that's useful to know.

---

_Comment by @zanieb on 2026-01-13 15:03_

> So for a broken environment, a uv sync will fix it? Say Python is upgraded from 3.12 to 3.13, will the environment continue using 3.12 or does the NO_MANAGED_PYTHON flag force it to upgrade?

I'm not sure what you mean by the `NO_MANAGED_PYTHON` part, but once you delete the Python installation how would uv continue to use 3.12?

You'll see something like..

```
warning: Ignoring existing virtual environment linked to non-existent Python interpreter: .venv/bin/python3 -> python
Using CPython 3.14.0
Removed virtual environment at: .venv
Creating virtual environment at: .venv
```

---

_Comment by @zoof on 2026-01-13 15:09_

What I'm wondering is whether uv would use 3.12 from https://github.com/astral-sh/python-build-standalone or if it would force a transition to 3.13.

---

_Comment by @zanieb on 2026-01-13 15:10_

We won't use a managed version if you have it disabled, no.

If you have a `.python-version` file pinning to 3.12 and 3.12 is no longer available, we'll just fail.

---

_Comment by @zoof on 2026-01-13 15:10_

I.e., is NO_MANAGED_PYTHON="true" a flag that can be overridden at the user level.

---

_Comment by @zoof on 2026-01-13 15:11_

Ok, thank you very much!

---

_Closed by @zanieb on 2026-01-13 15:13_

---
