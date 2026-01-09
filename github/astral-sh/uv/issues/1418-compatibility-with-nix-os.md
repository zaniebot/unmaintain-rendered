---
number: 1418
title: Compatibility with Nix(OS)?
type: issue
state: closed
author: gkze
labels:
  - duplicate
assignees: []
created_at: 2024-02-16T03:41:47Z
updated_at: 2024-02-16T14:55:41Z
url: https://github.com/astral-sh/uv/issues/1418
synced_at: 2026-01-07T13:12:16-06:00
---

# Compatibility with Nix(OS)?

---

_Issue opened by @gkze on 2024-02-16 03:41_

Hi, very excited to try this tool. Unfortunately Nix doesn't expose common executables on standard paths (`/bin/ls`, `/bin/ld`, etc.) so I'm not sure how to proceed. Any advice? I'm going to try to change the logic to perform a `$PATH`-based lookup, but not sure if absolute paths are desired. Would appreciate advice.

---

_Comment by @timleslie on 2024-02-16 03:45_

I believe this is a dupe of #1395.

---

_Comment by @charliermarsh on 2024-02-16 03:46_

Yeah I definitely want to fix this.

---

_Comment by @charliermarsh on 2024-02-16 04:08_

I'm gonna merge into #1395.

---

_Closed by @charliermarsh on 2024-02-16 04:08_

---

_Label `duplicate` added by @zanieb on 2024-02-16 14:55_

---
