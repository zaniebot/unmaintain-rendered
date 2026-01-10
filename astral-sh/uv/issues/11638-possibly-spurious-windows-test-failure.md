---
number: 11638
title: Possibly spurious Windows test failure
type: issue
state: open
author: jtfmumm
labels:
  - testing
  - windows
assignees: []
created_at: 2025-02-19T20:31:13Z
updated_at: 2025-02-19T20:31:25Z
url: https://github.com/astral-sh/uv/issues/11638
synced_at: 2026-01-10T01:25:08Z
---

# Possibly spurious Windows test failure

---

_Issue opened by @jtfmumm on 2025-02-19 20:31_

The test `uv::it sync::sync_default_groups` [failed for Windows](https://github.com/astral-sh/uv/actions/runs/13421544425/job/37495153143?pr=11593) on #11593, but it is unrelated to those changes. The exit_code was -1073741819. 

---

_Label `testing` added by @jtfmumm on 2025-02-19 20:31_

---

_Label `windows` added by @jtfmumm on 2025-02-19 20:31_

---
