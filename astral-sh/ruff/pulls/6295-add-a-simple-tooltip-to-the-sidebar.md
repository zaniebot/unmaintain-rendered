```yaml
number: 6295
title: Add a simple tooltip to the sidebar
type: pull_request
state: merged
author: charliermarsh
labels:
  - playground
assignees: []
merged: true
base: main
head: charlie/tooltip
created_at: 2023-08-03T01:31:53Z
updated_at: 2023-08-03T15:49:05Z
url: https://github.com/astral-sh/ruff/pull/6295
synced_at: 2026-01-12T02:52:03Z
```

# Add a simple tooltip to the sidebar

---

_Pull request opened by @charliermarsh on 2023-08-03 01:31_

## Summary

Not perfect, but IMO helpful:

<img width="1792" alt="Screen Shot 2023-08-02 at 9 29 24 PM" src="https://github.com/astral-sh/ruff/assets/1309177/e613e918-75cb-475e-9ea4-f833d1a0b5f6">

<img width="1792" alt="Screen Shot 2023-08-02 at 9 29 20 PM" src="https://github.com/astral-sh/ruff/assets/1309177/bb3efdfe-40e1-45b5-b774-082521b2d214">


---

_Label `playground` added by @charliermarsh on 2023-08-03 01:31_

---

_Merged by @charliermarsh on 2023-08-03 01:41_

---

_Closed by @charliermarsh on 2023-08-03 01:41_

---

_Branch deleted on 2023-08-03 01:41_

---

_Comment by @MichaReiser on 2023-08-03 06:36_

How's this different from what the browser renders by default?

![image](https://github.com/astral-sh/ruff/assets/1203881/bc999420-2bcf-4a83-b71e-a5dd240ca120)

Or does this only work on Firefox?

---

_Comment by @charliermarsh on 2023-08-03 15:44_

On Chrome I get this, but it requires ~two sustained seconds of hovering to appear:

<img width="225" alt="Screen Shot 2023-08-03 at 11 43 06 AM" src="https://github.com/astral-sh/ruff/assets/1309177/9423d24f-a249-42d2-8416-096e1a3cdca3">

I think instant feedback is _way_ better here, wdyt?


---

_Comment by @MichaReiser on 2023-08-03 15:49_

2 seconds seems long, agree. As long as it doesn't show up right under my cursor and steels the click event.

---
