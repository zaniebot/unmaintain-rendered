---
number: 12684
title: Python version accelerator
type: issue
state: closed
author: fishsixs
labels:
  - question
assignees: []
created_at: 2025-04-05T04:27:22Z
updated_at: 2025-04-07T19:10:06Z
url: https://github.com/astral-sh/uv/issues/12684
synced_at: 2026-01-07T13:12:18-06:00
---

# Python version accelerator

---

_Issue opened by @fishsixs on 2025-04-05 04:27_

### Question

UV manages the Python environment, and as far as I know, downloading can be accelerated by configuring the pip image source. But the Python version downloads slowly, how should I configure it

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @fishsixs on 2025-04-05 04:27_

---

_Comment by @FishAlchemist on 2025-04-05 04:49_

Do you need these two environment variables?
* [UV_PYPY_INSTALL_MIRROR](https://docs.astral.sh/uv/configuration/environment/#uv_pypy_install_mirror)
* [UV_PYTHON_INSTALL_MIRROR](https://docs.astral.sh/uv/configuration/environment/#uv_python_install_mirror)

---

_Comment by @fishsixs on 2025-04-05 07:13_

> 您需要这两个环境变量吗？
> 
> * [UV_PYPY_INSTALL_MIRROR](https://docs.astral.sh/uv/configuration/environment/#uv_pypy_install_mirror)
> * [UV_PYTHON_INSTALL_MIRROR](https://docs.astral.sh/uv/configuration/environment/#uv_python_install_mirror)
yes

---

_Closed by @charliermarsh on 2025-04-07 19:10_

---
