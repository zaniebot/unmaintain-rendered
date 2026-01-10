---
number: 14986
title: Need a mirror for github
type: issue
state: closed
author: fengzhao
labels:
  - enhancement
assignees: []
created_at: 2025-07-31T05:57:02Z
updated_at: 2025-07-31T11:49:42Z
url: https://github.com/astral-sh/uv/issues/14986
synced_at: 2026-01-10T01:25:51Z
---

# Need a mirror for github

---

_Issue opened by @fengzhao on 2025-07-31 05:57_

### Summary

You Know, for some reason , we can't connect github quickly , But  when I run `uv python install 3.10 3.11 3.12 3.13 3.14 ` , The UV will download [python-build-standalone](https://github.com/astral-sh/python-build-standalone) from the github , We need a parameter to se mirror for this , just like  ` https://ghfast.top/https://github.com/alibaba/DataX.git` 

### Example

_No response_

---

_Label `enhancement` added by @fengzhao on 2025-07-31 05:57_

---

_Comment by @FishAlchemist on 2025-07-31 07:47_

There are ways to set up mirroring. You can probably set it in either the global configuration file or as an environment variable.
* https://docs.astral.sh/uv/reference/environment/#uv_python_install_mirror
* https://docs.astral.sh/uv/reference/settings/#python-install-mirror

Speaking of the same issue, I'd like to promote my PR.
* https://github.com/astral-sh/uv/pull/14492

---

_Comment by @charliermarsh on 2025-07-31 11:49_

👍 We support using a mirror, see the environment variables linked above.

---

_Closed by @charliermarsh on 2025-07-31 11:49_

---

_Referenced in [astral-sh/uv#15191](../../astral-sh/uv/issues/15191.md) on 2025-08-11 12:12_

---
