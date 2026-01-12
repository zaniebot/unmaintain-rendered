```yaml
number: 14006
title: "Revert \"build-binaries for riscv64  (#12688)\""
type: pull_request
state: closed
author: Gankra
labels: []
assignees: []
base: main
head: gankra/revertv64
created_at: 2025-06-12T21:54:58Z
updated_at: 2025-06-13T13:57:11Z
url: https://github.com/astral-sh/uv/pull/14006
synced_at: 2026-01-12T16:10:58Z
```

# Revert "build-binaries for riscv64  (#12688)"

---

_@Gankra_

This reverts commit 210b5791880ea60700e0fb6513783efb10c3b086.

error: Failed to publish `wheels_uv_build/uv_build-0.7.13-py3-none-manylinux_2_31_riscv64.whl` to https://upload.pypi.org/legacy/
  Caused by: Upload failed with status code 400 Bad Request. Server says: 400 Binary wheel 'uv_build-0.7.13-py3-none-manylinux_2_31_riscv64.whl' has an unsupported platform tag 'manylinux_2_31_riscv64'.

---

_Comment by @Gankra on 2025-06-12 21:55_

cc @Xeonacid 

---

_Comment by @zanieb on 2025-06-12 22:00_

We should be able to publish these to GitHub still, but not PyPI.

---

_Comment by @Xeonacid on 2025-06-12 22:19_

Oops... Deeply apologize for that. 🙏

> We should be able to publish these to GitHub still, but not PyPI.

Could the team do that favor please? As the publish related actions is hard to test without an actual release, I'm scared of doing bad things again...
Thank you very much!

---

_Comment by @Gankra on 2025-06-12 22:20_

Additional context: pypi does not at all recognize riscv64 as an architecture for now, and we're not aware of any plans to add that.

---

_Closed by @Gankra on 2025-06-13 13:57_

---
