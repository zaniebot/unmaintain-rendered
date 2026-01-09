---
number: 3512
title: Wheel Build failures when installing via multiple simultaneous UV instances
type: issue
state: closed
author: yeswalrus
labels:
  - bug
assignees: []
created_at: 2024-05-10T17:15:09Z
updated_at: 2024-05-13T14:42:21Z
url: https://github.com/astral-sh/uv/issues/3512
synced_at: 2026-01-07T13:12:17-06:00
---

# Wheel Build failures when installing via multiple simultaneous UV instances

---

_Issue opened by @yeswalrus on 2024-05-10 17:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I've been attempting to switch our CI system over to UV, and after doing so we began encountering a relatively high failure rate. Investigating, it appears that there is a problem when multiple UV processes under the same user attempt to install in different venvs at once.

This can be reproduced with the following shell script on ubuntu 22 with any version of UV I tested with, up to 0.1.41.

```
#!/bin/bash
make_venv(){
    uv venv $1
    source $1/bin/activate
    uv pip install jaeger-client
}

for i in {1..8}
do
   make_venv $1/$i &
done
```
This inevitably produces failure messages like:
```
--- stderr:
error removing build/bdist.linux-x86_64/wheel/threadloop-1.0.2-py3.8.egg-info: [Errno 39] Directory not empty: 'build/bdist.linux-x86_64/wheel/threadloop-1.0.2-py3.8.egg-info'
error: [Errno 17] File exists: 'build/bdist.linux-x86_64/wheel/threadloop-1.0.2.dist-info'
```
It also appears to leave the uv cache in a corrupted state, where calling `uv cache clean` produces errors like:
```
Clearing cache at: ~/.cache/uv
error: Failed to clear cache at:~/.cache/uv
  Caused by: IO error for operation on ~/.cache/uv/built-wheels-v3/pypi/jaeger-client/4.8.0/0rRHp2ho5iISrFqrp0Brr/jaeger-client-4.8.0.tar.gz/build/bdist.linux-x86_64/wheel/jaeger_client: Permission denied (os error 13)
  Caused by: Permission denied (os error 13)
  ```
  This can be resolved by calling `chmod +rx` on whatever the offending file is and re-clearing the cache. This could perhaps be classified as a separate issue with failed builds leaving the cache in a corrupted state?
  

---

_Referenced in [astral-sh/uv#3514](../../astral-sh/uv/issues/3514.md) on 2024-05-10 17:59_

---

_Comment by @charliermarsh on 2024-05-10 18:08_

The first trace is an error in setuptools (not uv) which isn't safe to run across multiple processes. I can do some research, see if there's anything we can do there.

Can you file a separate issue for `uv cache clean`?


---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-10 18:12_

---

_Comment by @charliermarsh on 2024-05-10 18:12_

https://github.com/pypa/setuptools/issues/3119

---

_Comment by @charliermarsh on 2024-05-10 18:17_

One solution would be to add an advisory lock when building a source distribution. I'll do that.

---

_Label `bug` added by @charliermarsh on 2024-05-10 18:17_

---

_Comment by @charliermarsh on 2024-05-10 18:21_

The cache error is very confusing because we already have handling for that (we try to delete; if it fails and the file is read-only, we change permissions and try again).

---

_Referenced in [astral-sh/uv#3515](../../astral-sh/uv/issues/3515.md) on 2024-05-10 18:30_

---

_Comment by @yeswalrus on 2024-05-10 18:30_

> The first trace is an error in setuptools (not uv) which isn't safe to run across multiple processes. I can do some research, see if there's anything we can do there.
> 
> Can you file a separate issue for `uv cache clean`?

Created https://github.com/astral-sh/uv/issues/3515

---

_Comment by @charliermarsh on 2024-05-10 18:34_

Thank you!

---

_Comment by @yeswalrus on 2024-05-11 01:07_

FWIW I've been able to work around this problem by uploading prebuilt packages to our internal pypi index, but this still seems very much worth fixing!

---

_Comment by @charliermarsh on 2024-05-11 01:13_

Yup agreed!

---

_Referenced in [astral-sh/uv#3525](../../astral-sh/uv/pulls/3525.md) on 2024-05-11 17:18_

---

_Closed by @charliermarsh on 2024-05-13 14:42_

---
