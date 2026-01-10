---
number: 10373
title: uv pip install ignores python_requires in setup.py file
type: issue
state: closed
author: devashish2203
labels:
  - question
assignees: []
created_at: 2025-01-07T18:05:32Z
updated_at: 2025-01-07T18:48:43Z
url: https://github.com/astral-sh/uv/issues/10373
synced_at: 2026-01-10T01:24:53Z
---

# uv pip install ignores python_requires in setup.py file

---

_Issue opened by @devashish2203 on 2025-01-07 18:05_

# Steps to reproduce

In a new virtual env run the following

```
uv pip install --no-cache-dir "pybigquery>=0.5.0"
```
Notice pybigquery version 0.10.2 is installed. [verbose-output-uv.txt](https://github.com/user-attachments/files/18336706/verbose-output-uv.txt)

Repeat the same with pip
```
pip install --no-cache-dir "pybigquery>=0.5.0"
```
Notice that pybigquery version 0.5.0 is installed.


# Summary

When trying to debug the discrepancy I did not find anything [documented](https://docs.astral.sh/uv/pip/compatibility/) that explains the behaviour. 

Looking at the package itself https://pypi.org/project/pybigquery/0.6.0/#history. I noticed that v0.5.0 has no python_requires block  - https://github.com/googleapis/python-bigquery-sqlalchemy/blob/v0.5.0/setup.py 

while all versions after v0.5.0 and upto v0.10.2 have the python_requires https://github.com/googleapis/python-bigquery-sqlalchemy/blob/v0.10.0/setup.py#L74

Also note this python_requires block specifies <3.10. 
Since I'm on python 3.10.14, pip correctly resolves that the best match is v0.5.0, but uv ignores the python_requires and installs v0.10.2

# Versions

uv 0.5.15 (eb6ad9a4f 2025-01-06)
python 3.10.14
Operating System MacOS 14.6.1 (23G93)


Found https://github.com/astral-sh/uv/issues/2192 which was similar but this talks about local package installation only.

---

_Comment by @devashish2203 on 2025-01-07 18:09_

I guess a easier way to show the difference is to try to install pybigquery==0.6.0

```
> pip install --no-cache-dir "pybigquery==0.6.0"
ERROR: Ignored the following versions that require a different python version: 0.10.0 Requires-Python >=3.6, <3.10; 0.10.1 Requires-Python >=3.6, <3.10; 0.10.2 Requires-Python >=3.6, <3.10; 0.6.0 Requires-Python >=3.6, <3.10; 0.6.1 Requires-Python >=3.6, <3.10; 0.7.0 Requires-Python >=3.6, <3.10; 0.8.0 Requires-Python >=3.6, <3.10; 0.9.0 Requires-Python >=3.6, <3.10; 0.9.1 Requires-Python >=3.6, <3.10
ERROR: Could not find a version that satisfies the requirement pybigquery==0.6.0 (from versions: 0.2, 0.2.1, 0.2.3, 0.2.5, 0.2.6, 0.2.7, 0.2.8, 0.2.9, 0.3.0, 0.3.1, 0.3.2, 0.3.3, 0.3.4, 0.3.5, 0.4.0, 0.4.1, 0.4.2, 0.4.3, 0.4.4, 0.4.5, 0.4.6, 0.4.7, 0.4.8, 0.4.9, 0.4.10, 0.4.11, 0.4.12, 0.4.13, 0.4.14, 0.4.15, 0.5.0)
ERROR: No matching distribution found for pybigquery==0.6.0
```

Fails with pip, but works with uv pip
```
> uv pip install --no-cache-dir "pybigquery==0.6.0"
Using Python 3.10.14 environment at: /Users/devashish.chandra/.virtualenvs/test-env
Resolved 24 packages in 3.42s
   Built sqlalchemy==1.3.24
Prepared 23 packages in 17.62s
Installed 23 packages in 117ms
 + cachetools==4.2.4
 + certifi==2024.12.14
 + charset-normalizer==3.4.1
 + future==1.0.0
 + google-api-core==2.10.2
 + google-auth==1.35.0
 + google-cloud-bigquery==3.18.0
 + google-cloud-core==2.4.1
 + google-crc32c==1.6.0
 + google-resumable-media==2.7.2
 + googleapis-common-protos==1.66.0
 + idna==3.10
 + packaging==24.2
 + protobuf==4.25.5
 + pyasn1==0.6.1
 + pyasn1-modules==0.4.1
 + pybigquery==0.6.0
 + python-dateutil==2.9.0.post0
 + requests==2.32.3
 + rsa==4.9
 + six==1.17.0
 + sqlalchemy==1.3.24
 + urllib3==2.3.0
```

---

_Comment by @charliermarsh on 2025-01-07 18:18_

uv doesn't respect upper-bounds on Python requirements. I'm guessing that's what you're seeing here?

---

_Comment by @zanieb on 2025-01-07 18:18_

See https://github.com/astral-sh/uv/issues/4022 and linked issues.

---

_Label `question` added by @zanieb on 2025-01-07 18:18_

---

_Comment by @devashish2203 on 2025-01-07 18:43_

@charliermarsh Yes I believe that is what is happening.

@zanieb Thanks for the context. I don't fully understand the reasons for choosing to ignore the upper bounds on requires_python but at the very least should this be documented under https://docs.astral.sh/uv/pip/compatibility/#requires-python-enforcement since this is a difference in behaviour from pip?

---

_Comment by @zanieb on 2025-01-07 18:47_

It does seem reasonable to add, tracking in https://github.com/astral-sh/uv/issues/10376

---

_Comment by @devashish2203 on 2025-01-07 18:48_

Thanks. I'll close this issue then.

---

_Closed by @devashish2203 on 2025-01-07 18:48_

---
