---
number: 7066
title: "uv build doesn't respect SOURCE_DATE_EPOCH"
type: issue
state: closed
author: hynek
labels: []
assignees: []
created_at: 2024-09-05T04:42:52Z
updated_at: 2024-09-05T05:59:09Z
url: https://github.com/astral-sh/uv/issues/7066
synced_at: 2026-01-10T01:24:10Z
---

# uv build doesn't respect SOURCE_DATE_EPOCH

---

_Issue opened by @hynek on 2024-09-05 04:42_

While moving [build-and-inspect-package to uv build](https://github.com/hynek/build-and-inspect-python-package/pull/140), I noticed that you don't respect `SOURCE_DATE_EPOCH` (or something similar, but I believe that's a bit of a standard). *baipp* uses this to set the timestamp of all files to the timestamp of the latest commit, so this feature would be important for reproducible builds.

---

_Referenced in [hynek/build-and-inspect-python-package#140](../../hynek/build-and-inspect-python-package/pulls/140.md) on 2024-09-05 04:43_

---

_Comment by @hynek on 2024-09-05 05:59_

my bad, solved with the help of @henryiii, `uv build` is not responsible for any of this.

---

_Closed by @hynek on 2024-09-05 05:59_

---

_Referenced in [astral-sh/uv#13139](../../astral-sh/uv/issues/13139.md) on 2025-04-27 22:06_

---
