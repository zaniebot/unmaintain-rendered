---
number: 14920
title: "Missing read file permissions in wheel for license files and record with `uv_build` backend"
type: issue
state: closed
author: kieran-ryan
labels:
  - bug
assignees: []
created_at: 2025-07-27T17:27:16Z
updated_at: 2025-07-28T13:56:09Z
url: https://github.com/astral-sh/uv/issues/14920
synced_at: 2026-01-07T13:12:19-06:00
---

# Missing read file permissions in wheel for license files and record with `uv_build` backend

---

_Issue opened by @kieran-ryan on 2025-07-27 17:27_

### Summary

Using the `uv_build` backend, file permissions within the wheel distribution `.dist-info` directory for `RECORD` and the license file are `--w--wx---`; rather than `-rw-rw-r--` and `-rw-r--r--` respectively - as with `setuptools`; missing read permissions and having executable permissions. Reproduced locally and within a GitHub codespace.

Initialise a package and with a license file.

```console
uv init hello-world --package
cd hello-world
touch LICENSE
```

Reference the LICENSE file from the `project` section of the `pyproject.toml`.

```toml
license-files = ["LICENSE"]
```

Build the package and assess `RECORD` and `LICENSE` file permissions.

```console
> uv build
> unzip dist/*.whl
> ls -l hello_world-0.1.0.dist-info/RECORD hello_world-0.1.0.dist-info/licenses/LICENSE
--w--wx---  1 user  staff    0  1 Jan  1980 hello_world-0.1.0.dist-info/licenses/LICENSE
--w--wx---  1 user  staff  597  1 Jan  1980 hello_world-0.1.0.dist-info/RECORD
```

Identified while migrating from `setuptools` (cucumber/gherkin#439).

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.8.3 (7e78f54e7 2025-07-24)

### Python version

Python 3.13.2

---

_Label `bug` added by @kieran-ryan on 2025-07-27 17:27_

---

_Referenced in [astral-sh/uv#14930](../../astral-sh/uv/pulls/14930.md) on 2025-07-28 04:22_

---

_Assigned to @konstin by @zanieb on 2025-07-28 13:33_

---

_Comment by @zanieb on 2025-07-28 13:33_

Thanks for the well written report!

---

_Closed by @konstin on 2025-07-28 13:56_

---

_Referenced in [janosh/pymatviz#318](../../janosh/pymatviz/issues/318.md) on 2025-08-09 06:15_

---

_Referenced in [janosh/pymatviz#320](../../janosh/pymatviz/pulls/320.md) on 2025-08-14 09:49_

---

_Referenced in [jorenham/numpy-typing-compat#40](../../jorenham/numpy-typing-compat/issues/40.md) on 2025-08-14 20:09_

---

_Referenced in [ag2ai/faststream#2448](../../ag2ai/faststream/issues/2448.md) on 2025-08-14 20:17_

---

_Referenced in [jorenham/optype#411](../../jorenham/optype/issues/411.md) on 2025-08-15 15:24_

---

_Referenced in [pydantic/genai-prices#131](../../pydantic/genai-prices/pulls/131.md) on 2025-08-26 14:39_

---
