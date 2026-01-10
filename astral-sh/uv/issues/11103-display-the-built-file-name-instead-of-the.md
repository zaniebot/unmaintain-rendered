---
number: 11103
title: "Display the built file name instead of the canonicalized name in `uv build`"
type: issue
state: closed
author: dimbleby
labels:
  - bug
assignees: []
created_at: 2025-01-30T17:21:49Z
updated_at: 2025-02-20T08:11:17Z
url: https://github.com/astral-sh/uv/issues/11103
synced_at: 2026-01-10T01:25:01Z
---

# Display the built file name instead of the canonicalized name in `uv build`

---

_Issue opened by @dimbleby on 2025-01-30 17:21_

### Summary

eg
```bash
curl -O https://files.pythonhosted.org/packages/ea/46/f44d8be06b85bc7c4d8c95d658be2b68f27711f279bf9dd0612a5e4794f5/ruamel.yaml-0.18.10.tar.gz
uv build --wheel ruamel.yaml-0.18.10.tar.gz
```
giving output that ends `Successfully built ruamel_yaml-0.18.10-py3-none-any.whl`

but this is not true, the setuptools backend is not PEP491 compliant, so the file that is actually built is `ruamel.yaml-0.18.10-py3-none-any.whl`.

I guess uv is assuming that the output is always spec-compliant?

It's a pretty minor confusion.

(Also this particular example should go away soon, there's a pull request open in setuptools for PEP491-compliance that looks likely to merge any day now.)

### Platform

ubuntu 24.04

### Version

0.5.25

### Python version

3.13

---

_Label `bug` added by @dimbleby on 2025-01-30 17:21_

---

_Assigned to @konstin by @zanieb on 2025-01-30 17:23_

---

_Comment by @konstin on 2025-01-30 22:50_

Your guesses are right: uv is parsing the filename and using its normalized string printer for it. This would always be the same value for a spec-compliant build backend, which setuptools isn't (https://github.com/pypa/setuptools/pull/4766). We could warn about this but it's not something the user can fix (https://github.com/astral-sh/uv/pull/8203).

---

_Comment by @zanieb on 2025-01-31 18:34_

Why not display the unparsed file name? i.e., what does parsing / normalizing get us?

---

_Comment by @charliermarsh on 2025-01-31 18:35_

I don't think we even have it. We probably store the `WheelFilename`.

---

_Comment by @charliermarsh on 2025-01-31 18:36_

Oh nevermind, we do get it back from the build backend. We should just show the "real" filename.

---

_Comment by @konstin on 2025-01-31 19:15_

The parsing part gives us a correctness check that the output file is valid and not an error. We can change the message to verbatim independent of that.

---

_Renamed from "Files built by `uv build` do not necessarily match the "success" message" to "Display the built file name instead of the canonicalized name in `uv build`" by @zanieb on 2025-02-16 19:54_

---

_Unassigned @konstin by @konstin on 2025-02-18 08:42_

---

_Referenced in [astral-sh/uv#11593](../../astral-sh/uv/pulls/11593.md) on 2025-02-18 11:04_

---

_Referenced in [astral-sh/uv#11635](../../astral-sh/uv/issues/11635.md) on 2025-02-19 19:59_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-02-20 07:59_

---

_Closed by @jtfmumm on 2025-02-20 08:11_

---
