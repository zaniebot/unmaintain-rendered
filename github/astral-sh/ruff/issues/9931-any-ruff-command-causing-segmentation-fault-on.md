---
number: 9931
title: Any ruff command causing segmentation fault on Centos 7 with python 3.7.7
type: issue
state: closed
author: ShockwaveNN
labels:
  - bug
  - needs-info
assignees: []
created_at: 2024-02-11T14:46:02Z
updated_at: 2024-02-11T15:15:04Z
url: https://github.com/astral-sh/ruff/issues/9931
synced_at: 2026-01-07T13:12:15-06:00
---

# Any ruff command causing segmentation fault on Centos 7 with python 3.7.7

---

_Issue opened by @ShockwaveNN on 2024-02-11 14:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
```
$ ruff check .
Segmentation fault
$ ruff --version
Segmentation fault
$ python --version
Python 3.7.7
$ pip3 --version
pip 24.0 from ~/.local/lib/python3.7/site-packages/pip (python 3.7)
[plobashov@mtl-qauto-059 qa_auto_python_ufm_p]$ pip list | grep ruff
ruff                  0.2.1
[plobashov@mtl-qauto-059 qa_auto_python_ufm_p]$ cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

Any other ruff command also generate segmentation fault

I can provide more logs, but not sure which, please advice

---

_Comment by @charliermarsh on 2024-02-11 14:53_

Unfortunately I've never seen this before. Can you try a bisect to see if any older versions of Ruff work on Centos 7? 

---

_Label `bug` added by @charliermarsh on 2024-02-11 14:53_

---

_Label `needs-info` added by @charliermarsh on 2024-02-11 14:53_

---

_Comment by @ShockwaveNN on 2024-02-11 15:02_

Hm, for surprise ruff 0.1.9 (random version I picked first) works

Let me bisect for specific version

---

_Comment by @MichaReiser on 2024-02-11 15:02_

How did you install ruff? Did you use pip?

---

_Comment by @ShockwaveNN on 2024-02-11 15:04_

Hm guys thanks for your time but I think reinstalling latest version fixed the problem

Sorry for wasted time

```
$ ruff --version
ruff 0.2.1
$ ruff check .
....
results
...
```

---

_Closed by @ShockwaveNN on 2024-02-11 15:05_

---

_Comment by @charliermarsh on 2024-02-11 15:15_

No prob, that's good to hear!

---
