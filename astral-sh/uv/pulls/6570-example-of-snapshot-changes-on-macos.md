```yaml
number: 6570
title: Example of snapshot changes on macOS
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/snapshot-diff
created_at: 2024-08-24T05:56:24Z
updated_at: 2024-09-03T20:33:02Z
url: https://github.com/astral-sh/uv/pull/6570
synced_at: 2026-01-12T16:07:26Z
```

# Example of snapshot changes on macOS

---

_@zanieb_

Encountering these failures locally, suspicious of the test context changes.

But... if I go back to a959772074fbdf45eb00c8f6dfd7297e7377eaf0 this reproduces, actually. So I'm not sure what's going on here.

And... reproduces all the way back on 9d1cd8e48c534544ada11ac2c95832b7b687382f — something really wrong on my machine I guess?

---

_Label `internal` added by @zanieb on 2024-08-24 05:56_

---

_Comment by @zanieb on 2024-08-27 12:24_

Still encountering this. hm.

---

_Closed by @zanieb on 2024-09-03 20:32_

---

_Comment by @zanieb on 2024-09-03 20:33_

Needed to run `uv python install`

---
