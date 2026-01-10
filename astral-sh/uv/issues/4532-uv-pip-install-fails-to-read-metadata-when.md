---
number: 4532
title: "`uv pip install` fails to read metadata when running on Docker container due to failure to parse egg version"
type: issue
state: closed
author: kzvezdarov
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-06-25T21:16:27Z
updated_at: 2024-06-25T23:49:44Z
url: https://github.com/astral-sh/uv/issues/4532
synced_at: 2026-01-10T01:23:39Z
---

# `uv pip install` fails to read metadata when running on Docker container due to failure to parse egg version

---

_Issue opened by @kzvezdarov on 2024-06-25 21:16_

I'm using `uv` to provision a JupyterHub image with some additional packages. Up to `0.2.8`, my build worked fine, but starting with `0.2.9`, `uv pip install` fails with the following error:

```
(base) root@44dd6106b962:~# uv pip install --system --no-cache-dir --verbose aiohttp==3.9.5
DEBUG uv 0.2.15
DEBUG Searching for Python interpreter in system toolchains
DEBUG Found cpython 3.11.6 at `/opt/conda/bin/python3` (search path)
DEBUG Using Python 3.11.6 environment at /opt/conda/bin/python3
DEBUG Acquired lock for `/opt/conda`
error: Failed to read metadata: from /opt/conda/lib/python3.11/site-packages/pycurl-7.45.1-py3.11.egg-info
  Caused by: after parsing '7.45.1', found '-py3.11', which is not part of a valid version
```

Reverting to `0.2.8` fixes the issue and the installation process completes correctly.

### Minimal code snippet that reproduces the bug:
On a container, with this specific image:
```
docker run --rm -it --entrypoint=/bin/bash -u root jupyter/minimal-notebook:x86_64-python-3.11
```
 Run:
`pip install uv`

### Command:
```
(base) root@44dd6106b962:~# uv pip install --system --no-cache-dir --verbose aiohttp==3.9.5
DEBUG uv 0.2.15
DEBUG Searching for Python interpreter in system toolchains
DEBUG Found cpython 3.11.6 at `/opt/conda/bin/python3` (search path)
DEBUG Using Python 3.11.6 environment at /opt/conda/bin/python3
DEBUG Acquired lock for `/opt/conda`
error: Failed to read metadata: from /opt/conda/lib/python3.11/site-packages/pycurl-7.45.1-py3.11.egg-info
  Caused by: after parsing '7.45.1', found '-py3.11', which is not part of a valid version
```

#### Current uv platform:
Docker host: M1 MacOS, Container: `linux/amd64`

#### Current uv version (`uv --version`).
```
(base) root@645a2923df19:~# uv --version
uv 0.2.15
```

Looking at the changelog for 0.2.9, it is likely related to:
https://github.com/astral-sh/uv/pull/4082

---

_Renamed from "`uv pip install` fails to read metadata when running on Docker container" to "`uv pip install` fails to read metadata when running on Docker container due to failure to parse egg version" by @kzvezdarov on 2024-06-25 21:16_

---

_Comment by @charliermarsh on 2024-06-25 21:19_

Thanks, will take a look.

---

_Label `bug` added by @charliermarsh on 2024-06-25 21:21_

---

_Label `compatibility` added by @charliermarsh on 2024-06-25 21:21_

---

_Comment by @charliermarsh on 2024-06-25 21:21_

Likely from https://github.com/astral-sh/uv/pull/4082. Maybe I made a faulty assumption in the parsing?

---

_Comment by @charliermarsh on 2024-06-25 21:21_

Prior to v0.2.8, we probably just ignored those entirely and reinstall `pycurl`.

---

_Comment by @samypr100 on 2024-06-25 22:19_

Here's the egg-info metadata contents (not that it matters much since the issue is with the extra `-` in the .egg-info filename)

```
$ cat /opt/conda/lib/python3.11/site-packages/pycurl-7.45.1-py3.11.egg-info
Metadata-Version: 1.1
Name: pycurl
Version: 7.45.1
Summary: PycURL -- A Python Interface To The cURL library
Home-page: http://pycurl.io/
Author: Oleg Pudeyev
Author-email: oleg@bsdpower.com
License: LGPL/MIT
```

---

_Comment by @charliermarsh on 2024-06-25 22:43_

Ok yeah, egg links have optional components:

```python
EGG_NAME = re.compile(
    r"""
    (?P<name>[^-]+) (
        -(?P<ver>[^-]+) (
            -py(?P<pyver>[^-]+) (
                -(?P<plat>.+)
            )?
        )?
    )?
    """,
    re.VERBOSE | re.IGNORECASE,
).match
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-25 22:44_

---

_Comment by @samypr100 on 2024-06-25 22:51_

Just checked, but I think this can be fixed by adding this in the `path.is_file()` branch. It already exists in is_dir branch.

```rust
let Some((name, version_python)) = file_stem.split_once('-') else {
    return Ok(None);
};
let Some((version, _)) = version_python.split_once('-') else {
    return Ok(None);
};
```

---

_Comment by @charliermarsh on 2024-06-25 22:53_

I think it needs to flexibly handle either case? I'm gonna add a shared method and some tests.

---

_Referenced in [astral-sh/uv#4533](../../astral-sh/uv/pulls/4533.md) on 2024-06-25 23:15_

---

_Closed by @charliermarsh on 2024-06-25 23:49_

---

_Closed by @charliermarsh on 2024-06-25 23:49_

---
