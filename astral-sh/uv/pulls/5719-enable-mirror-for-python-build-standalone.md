```yaml
number: 5719
title: "Enable mirror for `python-build-standalone` downloads"
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: charlie/mirror
created_at: 2024-08-02T02:37:25Z
updated_at: 2024-08-08T01:34:21Z
url: https://github.com/astral-sh/uv/pull/5719
synced_at: 2026-01-12T16:06:58Z
```

# Enable mirror for `python-build-standalone` downloads

---

_@charliermarsh_

## Summary

This came up again recently, so decided to do it real quick.

Closes https://github.com/astral-sh/uv/issues/5224.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-02 02:37_

---

_Label `configuration` added by @charliermarsh on 2024-08-02 02:37_

---

_Label `preview` added by @charliermarsh on 2024-08-02 02:37_

---

_Marked ready for review by @charliermarsh on 2024-08-02 02:37_

---

_@charliermarsh reviewed on 2024-08-02 02:38_

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:418 on 2024-08-02 02:38_

PyPy downloads will fail this, but I think that's correct.

---

_@charliermarsh reviewed on 2024-08-02 02:40_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:240 on 2024-08-02 02:40_

![Screenshot 2024-08-01 at 10 40 02 PM](https://github.com/user-attachments/assets/67e74bf0-5af0-49f1-a658-77ca58825aca)


---

_@zanieb reviewed on 2024-08-05 17:46_

---

_Review comment by @zanieb on `README.md`:603 on 2024-08-05 17:46_

Sorry, but can you update `docs/configuration/environment.md` instead?

---

_@zanieb reviewed on 2024-08-05 17:47_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:418 on 2024-08-05 17:47_

So if you set the mirror and install PyPy it'll fail?

---

_@charliermarsh reviewed on 2024-08-05 18:00_

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:418 on 2024-08-05 18:00_

Yes

---

_@zanieb reviewed on 2024-08-05 18:25_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:418 on 2024-08-05 18:25_

Seems a little weird. I'm not sure what I'd expect though.

---

_@charliermarsh reviewed on 2024-08-07 03:04_

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:418 on 2024-08-07 03:04_

I could instead _also_ replace the PyPy URL, e.g., strip `"https://downloads.python.org` or `"https://downloads.python.org/pypy`.

---

_@zanieb reviewed on 2024-08-07 12:56_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:418 on 2024-08-07 12:56_

That might make sense to me, why did you avoid that in the first place?

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:418 on 2024-08-07 13:21_

Because the schemas are totally different, i.e., the thing that would come after your mirror URL. For CPython, you need a date, etc. For PyPy, it's just a filename.

---

_@charliermarsh reviewed on 2024-08-07 13:21_

---

_@zanieb reviewed on 2024-08-07 13:23_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:418 on 2024-08-07 13:23_

I see. So we'd need a separate `UV_PYPY_INSTALL_MIRROR` in the future? I'm okay with CPython being implicitly `PYTHON`.

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:418 on 2024-08-07 13:24_

(and so on for every additional implementation)

---

_@zanieb reviewed on 2024-08-07 13:24_

---

_@charliermarsh reviewed on 2024-08-08 01:27_

---

_Review comment by @charliermarsh on `README.md`:603 on 2024-08-08 01:27_

Done

---

_@charliermarsh reviewed on 2024-08-08 01:27_

---

_Review comment by @charliermarsh on `crates/uv-python/src/downloads.rs`:418 on 2024-08-08 01:27_

Added `UV_PYPY_INSTALL_MIRROR`.

---

_Merged by @charliermarsh on 2024-08-08 01:34_

---

_Closed by @charliermarsh on 2024-08-08 01:34_

---

_Branch deleted on 2024-08-08 01:34_

---
