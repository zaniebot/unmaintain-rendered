---
number: 16774
title: "`uv build` includes `uv` version in wheel"
type: issue
state: closed
author: davidbrochart
labels:
  - question
  - build-backend
assignees: []
created_at: 2025-11-19T11:02:16Z
updated_at: 2025-11-19T12:24:07Z
url: https://github.com/astral-sh/uv/issues/16774
synced_at: 2026-01-10T01:26:10Z
---

# `uv build` includes `uv` version in wheel

---

_Issue opened by @davidbrochart on 2025-11-19 11:02_

### Question

I'm using `uv build`/`uv publish` to publish to PyPI, as explained in the [documentation](https://docs.astral.sh/uv/guides/integration/github/#publishing-to-pypi). I'm relying on the fact that PyPI will ignore files that are already uploaded, if their hash is identical. But it seems that the `uv` version is included in the wheels. In the `package_name-package_version.dist-info/WHEEL` file I can see:
```
Wheel-Version: 1.0
Generator: uv 0.9.9
Root-Is-Purelib: true
Tag: py3-none-any
```
So it seems to me that whenever the `uv` version changes the hash of the wheel will change, and this will lead to an error when uploading, for instance:
```
error: Failed to publish `dist/fps_auth-0.9.4-py3-none-any.whl` to https://upload.pypi.org/legacy/
  Caused by: Upload failed with status code 400 Bad Request. Server says: 400 File already exists ('fps_auth-0.9.4-py3-none-any.whl', with blake2_256 hash '4125836b389d515f7a7f4a846ffbd73e1c1213ab7f15df3a1a56fac4e3480866'). See https://pypi.org/help/#file-name-reuse for more information.
```
Am I missing something?

### Platform

Ubuntu 24.04 x86_64

### Version

0.9.9

---

_Label `question` added by @davidbrochart on 2025-11-19 11:02_

---

_Comment by @konstin on 2025-11-19 11:12_

That's correct, but that problem shouldn't be unique to uv, but occur with most build backends (and independent of the build frontend), which write their version in the the `WHEEL` file. In other ecosystems that don't support the ignore behavior PyPI has, specific upload triggers are commonly used, such as a git tag for small projects with a single project in the repo or GitHub Actions workflow triggers for more complex projects.

---

_Label `build-backend` added by @konstin on 2025-11-19 11:13_

---

_Comment by @davidbrochart on 2025-11-19 11:32_

Thanks for the answer, yes I saw that `hatchling` included its version too.
After reading #7917, I understand that I was relying on a feature that `uv` decided to not support.

---

_Comment by @davidbrochart on 2025-11-19 12:24_

For now I'm using the workaround in https://github.com/astral-sh/uv/issues/7917#issuecomment-3552425346.

---

_Closed by @davidbrochart on 2025-11-19 12:24_

---
