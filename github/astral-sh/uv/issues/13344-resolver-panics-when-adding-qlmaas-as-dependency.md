---
number: 13344
title: Resolver panics when adding qlmaas as dependency
type: issue
state: closed
author: shumpohl
labels:
  - bug
assignees: []
created_at: 2025-05-08T10:32:00Z
updated_at: 2025-05-13T03:03:45Z
url: https://github.com/astral-sh/uv/issues/13344
synced_at: 2026-01-07T13:12:18-06:00
---

# Resolver panics when adding qlmaas as dependency

---

_Issue opened by @shumpohl on 2025-05-08 10:32_

### Summary

To reproduce execute `uv init test_env`, `cd test_env`, `uv add qlmaas`.

On windows:

```powershell
PS > uv --version
uv 0.7.3 (3c413f74b 2025-05-07)
```

```powershell
PS > uv add myqlm
Using CPython 3.12.9 interpreter at: C:\Users\%USER%\AppData\Local\Programs\Python\Python312\python.exe
Creating virtual environment at: .venv
⠼ qlmaas==1.11.2
thread 'uv-resolver' panicked at C:\Users\runneradmin\.cargo\git\checkouts\pubgrub-e6d66ebc0e28e95d\a3b4db3\src\term.rs:89:18:
Positive term cannot unwrap negative set
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
error: The channel closed unexpectedly
```

And on debian:

```bash
$ uv --version
uv 0.7.3
```

```bash
$ uv add qlmaas
Using CPython 3.11.12
Creating virtual environment at: .venv
⠙ qlmaas==1.11.2
thread 'uv-resolver' panicked at /root/.cargo/git/checkouts/pubgrub-e6d66ebc0e28e95d/a3b4db3/src/term.rs:89:18:
Positive term cannot unwrap negative set
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
error: The channel closed unexpectedly
```

### Platform

Linux 6.12.21-amd64 x86_64 GNU/Linux

### Version

uv 0.7.3

### Python version

3.11 and 3.12

---

_Label `bug` added by @shumpohl on 2025-05-08 10:32_

---

_Referenced in [pubgrub-rs/pubgrub#337](../../pubgrub-rs/pubgrub/pulls/337.md) on 2025-05-08 10:43_

---

_Comment by @shumpohl on 2025-05-08 10:49_

The problem seems to be caused by the wheel's METADATA containing itself multiple times as a dependency:

```
Metadata-Version: 2.2
Name: qlmaas
Version: 1.11.2
Summary: Module qlmaas [ecea8019] - Compiled by Eviden
Home-page: https://atos.net/en/lp/myqlm
Author: Eviden Quantum Lab
Author-email: myqlm@atos.net
License: Eviden myQLM EULA
Project-URL: Documentation, https://myqlm.github.io
Project-URL: Bug Tracker, https://github.com/myQLM/myqlm-issues/issues
Project-URL: Community, https://myqlmworkspace.slack.com
Keywords: Quantum,myQLM,QLM,Atos,Eviden
Platform: Operating System :: Microsoft :: Windows
Classifier: Operating System :: Microsoft :: Windows
Classifier: Programming Language :: Python :: 3.1
Classifier: Topic :: Scientific/Engineering
Classifier: License :: Other/Proprietary License
Description-Content-Type: text/markdown
License-File: LICENSE.txt
Requires-Dist: dill
Requires-Dist: prompt_toolkit
Requires-Dist: prettytable
Requires-Dist: pyOpenSSL
Requires-Dist: cffi
Requires-Dist: qat-hardware>=1.5.0
Requires-Dist: qat-quops>=1.4.0
Requires-Dist: packaging
Requires-Dist: qat-comm
Requires-Dist: qat-core
Requires-Dist: qat-hardware
Requires-Dist: qat-lang
Requires-Dist: qat-quops
Requires-Dist: qlmaas
Requires-Dist: qlmaas
Requires-Dist: qlmaas
Dynamic: author
Dynamic: author-email
Dynamic: classifier
Dynamic: description
Dynamic: description-content-type
Dynamic: home-page
Dynamic: keywords
Dynamic: license
Dynamic: platform
Dynamic: project-url
Dynamic: requires-dist
Dynamic: summary

<-- snip -->
```

If I delete 2 of the 3 `Requires-Dist: qlmaas` lines and repackage the wheel, the installation completes without problems.

I tested this on windows. The wheel was apparently packaged with setuptools as the `WHEEL` file reads

```
Wheel-Version: 1.0
Generator: setuptools (75.8.0)
Root-Is-Purelib: true
Tag: cp312-cp312-win_amd64
```

---

_Referenced in [myQLM/myqlm-issues#40](../../myQLM/myqlm-issues/issues/40.md) on 2025-05-08 11:00_

---

_Assigned to @konstin by @konstin on 2025-05-09 14:10_

---

_Referenced in [astral-sh/uv#13366](../../astral-sh/uv/pulls/13366.md) on 2025-05-09 16:32_

---

_Comment by @konstin on 2025-05-09 16:41_

Thank you for the clear bug report and the investigation

---

_Closed by @charliermarsh on 2025-05-13 03:03_

---
