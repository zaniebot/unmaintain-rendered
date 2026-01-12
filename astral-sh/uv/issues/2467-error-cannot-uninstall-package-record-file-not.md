```yaml
number: 2467
title: "error: Cannot uninstall package; RECORD file not found "
type: issue
state: closed
author: ksamuel
labels:
  - needs-decision
assignees: []
created_at: 2024-03-14T21:44:28Z
updated_at: 2024-03-20T01:02:13Z
url: https://github.com/astral-sh/uv/issues/2467
synced_at: 2026-01-12T15:58:37Z
```

# error: Cannot uninstall package; RECORD file not found 

---

_@ksamuel_

Trying to install a package with uv stops because the RECORD file for a dependency package it needs to uninstall does not exist. This means if a command goes wrong, the venv is corrupted for good an can't be fixed without manual intervention.

E.G:

```
uv pip install wagtail-localize 
Resolved 36 packages in 6ms
error: Cannot uninstall package; RECORD file not found at: /.../.venv/lib/python3.11/site-packages/wagtail-6.0.1.dist-info/RECORD
```

And indeed /.../.venv/lib/python3.11/site-packages/wagtail-6.0.1.dist-info/ is empty, because some previous uninstallation failed and left the env in a dirty state.

Interestingly, I use only uv on this project, which means it is also responsible for the empty dir. This is another problem to solve but I cannot reproduce it reliably.


Ubuntu 20.04, Python 3.11.8, uv 0.1.11


---

_Comment by @charliermarsh on 2024-03-14 22:01_

Hmm, yeah. This is the correct behavior though as described in the spec:

> If the RECORD file is missing, tools that rely on .dist-info must not attempt to uninstall or upgrade the package. (This restriction does not apply to tools that rely on other sources of information, such as system package managers in Linux distros.)

I don't know that it's wrong to require some manual intervention in this case -- the environment _is_ broken. (In general, I think virtualenvs should be viewed as completely disposable. Re-creating the virtualenv is effectively free with uv.)

I think the alternative would be that we just ignore the package entirely and overwrite the contents when we go to install, but that also risks leaving your environment in a broken state. We could just proceed-but-warn, I suppose?


---

_Label `needs-decision` added by @charliermarsh on 2024-03-15 02:40_

---

_Comment by @charliermarsh on 2024-03-15 03:18_

I can change this to a warning.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-15 03:18_

---

_Closed by @charliermarsh on 2024-03-20 01:02_

---
