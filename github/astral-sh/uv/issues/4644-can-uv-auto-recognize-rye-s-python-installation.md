---
number: 4644
title: "Can `uv` auto recognize `rye`'s Python installation?"
type: issue
state: open
author: jfcherng
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-06-29T06:35:58Z
updated_at: 2024-07-31T18:33:53Z
url: https://github.com/astral-sh/uv/issues/4644
synced_at: 2026-01-07T13:12:17-06:00
---

# Can `uv` auto recognize `rye`'s Python installation?

---

_Issue opened by @jfcherng on 2024-06-29 06:35_

For example, I have `C:\Users\jfcherng\.rye\py\cpython@3.8.19` but it won't be listed on `uv toolchain list` and I can't do `uv venv --python 3.8`. I can add `C:\Users\jfcherng\.rye\py\cpython@3.8.19` (and other installations) into `PATH` env variable but installation directories are bound to `major.min.patch` version. That means I have to update `PATH` every time when there is a `patch` version update.

I know there is `uv toolchain install` but that feels like a duplication to me since I already have a copy under `C:\Users\jfcherng\.rye\py`.

Just wondering how do people do it right?

---

_Comment by @zanieb on 2024-06-29 14:49_

I think we're unlikely to recognize toolchains provided by other tools, but I feel your pain here. We could probably make `UV_TOOLCHAIN_DIR` support `C:\Users\jfcherng\.rye\py`, it'd take some tweaks to our toolchain install parsing and opt-in but seems relatively reasonable.

---

_Label `needs-decision` added by @zanieb on 2024-07-01 21:27_

---

_Label `configuration` added by @zanieb on 2024-07-01 21:27_

---

_Comment by @zanieb on 2024-07-01 21:27_

cc @charliermarsh, thoughts?

---

_Comment by @bluss on 2024-07-23 05:57_

Maybe rye can migrate to just reusing uv's toolchain feature (?) then it would be unified that way instead.

---

_Comment by @jfcherng on 2024-07-23 06:20_

> Maybe rye can migrate to just reusing uv's toolchain feature (?) then it would be unified that way instead.

Ah, yes, since `rye` uses `uv`, that may make more sense.


---
