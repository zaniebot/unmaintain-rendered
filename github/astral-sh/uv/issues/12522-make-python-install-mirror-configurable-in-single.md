---
number: 12522
title: "Make `python-install-mirror` configurable in single-file scripts"
type: issue
state: open
author: l1xnan
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-03-28T03:39:14Z
updated_at: 2025-07-31T06:07:09Z
url: https://github.com/astral-sh/uv/issues/12522
synced_at: 2026-01-07T13:12:18-06:00
---

# Make `python-install-mirror` configurable in single-file scripts

---

_Issue opened by @l1xnan on 2025-03-28 03:39_

### Summary

Constrained by the network environment, I hope that the single-file script I distribute includes `python-install-mirror` configuration. In this way, when users execute the script, Python and its dependencies can be automatically installed without being restricted by the network, and the configuration can be completed. There is no need to specifically emphasize to the script users that they need to configure the `UV_PYTHON_INSTALL_MIRROR` environment variable, which will provide a smoother user experience.

```python
# /// script
# requires-python = ">=3.10,<3.11"
# dependencies = [
#     "requests<3",
#     "rich",
# ]
# [[tool.uv.index]]
# url = "https://pypi.tuna.tsinghua.edu.cn/simple"

# [tool.uv]
# python-install-mirror = "https://<mirror-url>/indygreg/python-build-standalone/releases/download"
# ///
import requests
```

However, currently, the metadata of the single-file script and the command line `uv run script.py --python-install-mirror="https://<python-install-mirror>"` do not support the `python-install-mirror` configuration. I hope this configuration can be supported.  

### Example

_No response_

---

_Label `enhancement` added by @l1xnan on 2025-03-28 03:39_

---

_Referenced in [astral-sh/uv#12556](../../astral-sh/uv/pulls/12556.md) on 2025-03-30 09:19_

---

_Comment by @zanieb on 2025-04-01 19:35_

We've previously considered this a user-setting, not a project or script setting. I would be surprised if I ran a script and it transparently fetched Python versions from a mirror. 

---

_Label `needs-decision` added by @zanieb on 2025-04-01 19:35_

---

_Comment by @l1xnan on 2025-04-02 01:37_

> We've previously considered this a user-setting, not a project or script setting. I would be surprised if I ran a script and it transparently fetched Python versions from a mirror.

I agree that in the vast majority of cases, it is a user setting, and for most people, configuring it using environment variables is a better choice. My usage scenario is rather special. I need to share running scripts with people who are not familiar with programming in an environment with network limitations, and I hope to minimize the communication costs related to network issues. In addition, it can make the configurations in the single-file script as consistent as possible with the available configurations in pyproject.toml. If it is finally considered that it is better to only place it in the user settings, I am also very willing to accept that. In that case, I will specifically emphasize the configuration of the environment variables during the stage of guiding the installation of uv. Thank you for your explanation!  

---

_Comment by @fengzhao on 2025-07-31 06:07_

> > We've previously considered this a user-setting, not a project or script setting. I would be surprised if I ran a script and it transparently fetched Python versions from a mirror.
> 
> I agree that in the vast majority of cases, it is a user setting, and for most people, configuring it using environment variables is a better choice. My usage scenario is rather special. I need to share running scripts with people who are not familiar with programming in an environment with network limitations, and I hope to minimize the communication costs related to network issues. In addition, it can make the configurations in the single-file script as consistent as possible with the available configurations in pyproject.toml. If it is finally considered that it is better to only place it in the user settings, I am also very willing to accept that. In that case, I will specifically emphasize the configuration of the environment variables during the stage of guiding the installation of uv. Thank you for your explanation!

Agree

---
