```yaml
number: 12441
title: "`uv sync` does not respect `build-constraint-dependencies`"
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-03-24T17:03:31Z
updated_at: 2025-08-27T19:48:06Z
url: https://github.com/astral-sh/uv/issues/12441
synced_at: 2026-01-10T03:23:53Z
```

# `uv sync` does not respect `build-constraint-dependencies`

---

_Issue opened by @zanieb on 2025-03-24 17:03_

While debugging #12440, we noticed this setting was not properly propagated. These constraints _are_ respected when using `uv pip`.

---

_Comment by @h0rv on 2025-03-24 17:13_

For reference, a workaround: 

```bash
uv export > requirements.txt
uv venv
uv pip install -r requirements.txt
```

Also need to set build constraints: https://docs.astral.sh/uv/reference/settings/#build-constraint-dependencies

---

_Label `bug` added by @charliermarsh on 2025-03-24 18:56_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-24 18:56_

---

_Comment by @inoa-jboliveira on 2025-03-25 14:38_

Hi everyone, I know this is no longer very high priority since setuptools rolled back the issue yesterday, but I would like to make sure I can control the version used to build some old libraries via uv sync.

Just checking if there is a fix or workaround for sync (not pip install).

---

_Comment by @zanieb on 2025-03-25 14:39_

It's still a high priority for us.

---

_Closed by @charliermarsh on 2025-03-27 21:11_

---

_Closed by @charliermarsh on 2025-03-27 21:11_

---

_Comment by @jamied-treatmachine on 2025-08-27 19:04_

This seems to still be broken in 0.8.13... what am I doing wrong? Below is an example pyproject.toml and my shell transcipt for creating it. My goal is to get rid of the setuptools deprecation warnings in newer versions. QUICK EDIT: running `uv sync` doesn't change the situation at all.

```toml
[project]
name = "uv-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "snowflake-snowpark-python==1.36.0",
]

[tool.uv]
build-constraint-dependencies = [
    "setuptools==60.0.0",
]
```

```
[11:57:02 ~/Projects]$ mkdir uv-test
[11:57:10 ~/Projects]$ cd uv-test
[11:57:14 ~/Projects/uv-test]$ uv --version
uv 0.8.13 (ede75fe62 2025-08-21)
[11:57:27 ~/Projects/uv-test]$ uv init
Initialized project `uv-test`
[11:57:45 ~/Projects/uv-test]$ echo '
[tool.uv]
build-constraint-dependencies = [
    "setuptools==60.0.0",
]
' >> pyproject.toml
[11:57:45 ~/Projects/uv-test]$ uv add snowflake-snowpark-python==1.36.0
Using CPython 3.10.9
Creating virtual environment at: .venv
Resolved 34 packages in 57ms
Installed 32 packages in 86ms
 + asn1crypto==1.5.1
 + boto3==1.40.18
 + botocore==1.40.18
 + certifi==2025.8.3
 + cffi==1.17.1
 + charset-normalizer==3.4.3
 + cloudpickle==3.0.0
 + cryptography==45.0.6
 + filelock==3.19.1
 + idna==3.10
 + jmespath==1.0.1
 + packaging==25.0
 + platformdirs==4.4.0
 + protobuf==5.29.5
 + pycparser==2.22
 + pyjwt==2.10.1
 + pyopenssl==25.1.0
 + python-dateutil==2.9.0.post0
 + pytz==2025.2
 + pyyaml==6.0.2
 + requests==2.32.5
 + s3transfer==0.13.1
 + setuptools==80.9.0
 + six==1.17.0
 + snowflake-connector-python==3.17.2
 + snowflake-snowpark-python==1.36.0
 + sortedcontainers==2.4.0
 + tomlkit==0.13.3
 + typing-extensions==4.15.0
 + tzlocal==5.3.1
 + urllib3==2.5.0
 + wheel==0.45.1
[12:02:45 ~/Projects/uv-test]$ uv pip show setuptools
Name: setuptools
Version: 80.9.0
Location: /Users/jamie/Projects/uv-test/.venv/lib/python3.10/site-packages
Requires:
Required-by: snowflake-snowpark-python
```


---

_Comment by @zanieb on 2025-08-27 19:22_

It's a constraint on _build_ dependencies, i.e., packages for isolated build environments. It looks like there you just have a transitive _runtime_ dependency on setuptools? In which case, you want a normal constraint.

---

_Comment by @jamied-treatmachine on 2025-08-27 19:47_

> It's a constraint on _build_ dependencies, i.e., packages for isolated build environments. It looks like there you just have a transitive _runtime_ dependency on setuptools? In which case, you want a normal constraint.

Thank you! this worked, hopefully it will help other noobs confused by the two options
```
[12:46:30 ~/Projects/uv-test]$ uv sync
Resolved 34 packages in 494ms
Prepared 1 package in 183ms
Uninstalled 1 package in 63ms
Installed 1 package in 7ms
 - setuptools==80.9.0
 + setuptools==60.0.0
```


---
