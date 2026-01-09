---
number: 4748
title: "Feature Request: Include version updating from when running `uv self update`"
type: issue
state: closed
author: notatallshaw
labels:
  - tracing
  - cli
assignees: []
created_at: 2024-07-02T22:57:58Z
updated_at: 2024-08-23T13:50:29Z
url: https://github.com/astral-sh/uv/issues/4748
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature Request: Include version updating from when running `uv self update`

---

_Issue opened by @notatallshaw on 2024-07-02 22:57_

Very minor quality of life request, when you run `uv self update` it gives you an output like:

```
info: Checking for updates...
success: Upgraded `uv` to v0.2.20! https://github.com/astral-sh/uv/releases/tag/0.2.20
```

But it never told me what version I updated from, sometimes when I'm logging into an environment I've not logged into for a little bit I don't remember what uv version I was on.

Mostly this is a personal preference, but occasionally it's a little harder to identify where a fix or a regression happened because I don't know one of my two data points.

---

_Comment by @zanieb on 2024-07-02 23:01_

If you use `uv self update -v` we'll display the current version before doing anything (and not include much else). Fair request though!

---

_Label `tracing` added by @zanieb on 2024-07-02 23:01_

---

_Label `cli` added by @zanieb on 2024-07-02 23:01_

---

_Referenced in [astral-sh/uv#6473](../../astral-sh/uv/pulls/6473.md) on 2024-08-23 13:45_

---

_Closed by @zanieb on 2024-08-23 13:50_

---
