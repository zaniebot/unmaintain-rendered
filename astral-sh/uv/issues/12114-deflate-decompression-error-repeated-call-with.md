---
number: 12114
title: "deflate decompression error: repeated call with bad state"
type: issue
state: closed
author: nijel
labels:
  - error messages
assignees: []
created_at: 2025-03-11T13:06:39Z
updated_at: 2025-03-14T00:29:57Z
url: https://github.com/astral-sh/uv/issues/12114
synced_at: 2026-01-10T01:25:15Z
---

# deflate decompression error: repeated call with bad state

---

_Issue opened by @nijel on 2025-03-11 13:06_

### Summary

Recently, I started to occasionally get `deflate decompression error: repeated call with bad state` errors on uv sync. My guess is that it is caused by some temporary networking issue (this happens in GitHub Actions), but the error message is not really helpful to diagnose the root cause. Maybe the error handling could be better?

Full log:

```
 Using CPython 3.12.9 interpreter at: /opt/hostedtoolcache/Python/3.12.9/x64/bin/python3
Creating virtual environment at: .venv
Resolved 84 packages in 1.64s
   Building translate-toolkit @ file:///home/runner/work/translate/translate
Downloading virtualenv (4.1MiB)
Downloading rapidfuzz (3.0MiB)
Downloading kiwisolver (1.4MiB)
Downloading pygments (1.2MiB)
Downloading pillow (4.3MiB)
Downloading numpy (15.4MiB)
Downloading babel (9.7MiB)
Downloading lxml (4.8MiB)
Downloading matplotlib (8.2MiB)
Downloading fonttools (4.6MiB)
Downloading sphinx (3.4MiB)
Downloading setuptools (1.2MiB)
  × Failed to download `virtualenv==20.29.3`
  ├─▶ Failed to extract archive
  ╰─▶ deflate decompression error: repeated call with bad state
  help: `virtualenv` (v20.29.3) was included because `translate-toolkit:dev`
        depends on `pre-commit` (v4.1.0) which depends on `virtualenv`
```

Workflow log: https://github.com/translate/translate/actions/runs/13786806477/job/38556653760?pr=5535

### Platform

Ubuntu 24.04.2 amd64 (GitHub Actions)

### Version

 0.6.5

### Python version

3.12.9

---

_Label `bug` added by @nijel on 2025-03-11 13:06_

---

_Comment by @konstin on 2025-03-11 13:40_

Could this be the same error as #8692 / https://status.python.org/incidents/2zf08fkp0nd1 ?

---

_Comment by @jayqi on 2025-03-11 13:44_

I also had some similar installation issues about 8-9 hours ago (around 04:00–05:00 UTC) with uv in GitHub Actions.

---

## Case 1

Running: `uvx nox -s tests-3.12 --verbose `

Download/install fails when setting up nox as a tool.

```
Downloading virtualenv (4.1MiB)
  × Failed to download `virtualenv==20.29.3`
  ├─▶ Failed to extract archive
  ╰─▶ deflate decompression error: repeated call with bad state
  help: `virtualenv` (v20.29.3) was included because `nox` (v2025.2.9) depends
        on `virtualenv`
Error: Process completed with exit code 1.
```
https://github.com/drivendataorg/erdantic/actions/runs/13780302271/job/38537257570#step:5:13

- astral-sh/setup-uv@v5
- uv version 0.6.5
- ubuntu-latest (ubuntu-24.04, 20250302.1.0)

---

## Case 2

Download/install fails when installing with `uv pip`.

```
nox > /opt/hostedtoolcache/uv/0.6.5/x86_64/uv pip install -r requirements/tests.txt
Using Python 3.13.2 environment at: .nox/tests-3-13
Resolved 42 packages in 149ms
   Building pygraphviz==1.14
   Building erdantic @ file:///home/runner/work/erdantic/erdantic
Downloading pydantic-core (1.9MiB)
Downloading jedi (1.5MiB)
Downloading pygments (1.2MiB)
  × Failed to download `pydantic-core==2.27.2`
  ├─▶ Failed to extract archive
  ╰─▶ unexpected BufError
  help: `pydantic-core` (v2.27.2) was included because `erdantic` (v1.0.5)
        depends on `pydantic-core`
nox > Command /opt/hostedtoolcache/uv/0.6.5/x86_64/uv pip install -r requirements/tests.txt failed with exit code 1
```

https://github.com/drivendataorg/erdantic/actions/runs/13780431633/job/38537603774#step:5:290

- astral-sh/setup-uv@v5
- uv version 0.6.5
- ubuntu-latest (ubuntu-24.04, 20250309.1.0)

---

_Comment by @charliermarsh on 2025-03-11 13:47_

I believe this is due to a PyPI outage?

---

_Comment by @nijel on 2025-03-11 13:51_

Yes, that could likely be the cause. Still, the message looks like there is some issue in error handling. But please don't hesitate to close this if you think it is okay.

---

_Comment by @charliermarsh on 2025-03-14 00:29_

I think the message could be better but going to close for now since the behavior seems alright.

---

_Closed by @charliermarsh on 2025-03-14 00:29_

---

_Label `error messages` added by @charliermarsh on 2025-03-14 00:29_

---

_Label `bug` removed by @charliermarsh on 2025-03-14 00:29_

---
