---
number: 1420
title: "Install `pip` by default in new environments"
type: issue
state: closed
author: ikrommyd
labels:
  - configuration
  - virtualenv
assignees: []
created_at: 2024-02-16T03:55:38Z
updated_at: 2024-04-07T00:29:35Z
url: https://github.com/astral-sh/uv/issues/1420
synced_at: 2026-01-07T13:12:16-06:00
---

# Install `pip` by default in new environments

---

_Issue opened by @ikrommyd on 2024-02-16 03:55_

I think it's more logical to automatically install pip in the newly created environments by `uv venv` so that the user can immediately have access to pip within the new environent. Now if you create a new environment and activate it, pip points to the system pip.

Please close if this is a duplicate issue that I wasn't able to find

---

_Comment by @charliermarsh on 2024-02-16 04:07_

Just in case you didn't see: you can run `uv venv --seed` to include `pip`, `wheel`, and `setuptools`.

---

_Comment by @charliermarsh on 2024-02-16 04:07_

(Whether that should be the default is a reasonable question, so I'll leave this open! But wanted to make sure you knew about that option.)

---

_Label `configuration` added by @charliermarsh on 2024-02-16 04:08_

---

_Comment by @ikrommyd on 2024-02-16 04:12_

Nah, I didn't know it. Thanks! I was just comparing `uv venv` with `virtualenv .venv` and what those install to the new environment

---

_Comment by @charliermarsh on 2024-02-16 04:15_

Yeah, we exclude these "seed" packages by default, since in theory you don't need them if you use `uv` to handle package installation. But if you're using `uv` to create virtualenvs that you want to manipulate with `pip`, you do need `--seed`.

---

_Comment by @zanieb on 2024-02-16 05:03_

See #1330 as well

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:38_

---

_Comment by @charliermarsh on 2024-04-07 00:17_

I'm happy with our current choice here, though I know it can take some adjustment for users.

---

_Closed by @charliermarsh on 2024-04-07 00:17_

---

_Comment by @ikrommyd on 2024-04-07 00:21_

Yup, I should have actually closed it myself, I had just forgotten about it.

---

_Comment by @charliermarsh on 2024-04-07 00:29_

All good, thanks for following up :)

---
