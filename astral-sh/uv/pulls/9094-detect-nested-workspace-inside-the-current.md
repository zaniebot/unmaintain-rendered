```yaml
number: 9094
title: Detect nested workspace inside the current workspace and members with identical names
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: micha/workspace-discovery-nested
created_at: 2024-11-13T18:13:41Z
updated_at: 2024-11-15T04:03:01Z
url: https://github.com/astral-sh/uv/pull/9094
synced_at: 2026-01-12T16:08:38Z
```

# Detect nested workspace inside the current workspace and members with identical names

---

_@MichaReiser_

## Summary

Align uv's workspace discovery with red knots (see https://github.com/astral-sh/ruff/pull/14308#issuecomment-2474296308)

* Detect nested workspace inside the current workspace rather than testing if the current workspace is a member of any outer workspace. 
* Detect packages with identical names.

## Test Plan

I added two integration tests. I also back ported the tests to main to verify that both these invalid workspaces aren't catched by uv today. That makes this, technically, a breaking change but I would consider the change a bug fix.

---

_Comment by @charliermarsh on 2024-11-13 18:15_

Thanks king! I should've clarified that I was also willing to do this if we shipped the version you have in red-knot, but I'm grateful to you for PRing it.

---

_Comment by @charliermarsh on 2024-11-13 18:15_

I can do the testing etc. if you need.

---

_@MichaReiser reviewed on 2024-11-13 18:17_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yml`:26 on 2024-11-13 18:17_

huh, why?

---

_Comment by @MichaReiser on 2024-11-13 18:20_

I can do it tomorrow but let me know if you have any suggestions on how/where to add automated tests for this (considering that no tests are failing, it seems there are no existing tests)

---

_Marked ready for review by @MichaReiser on 2024-11-14 08:52_

---

_@charliermarsh approved on 2024-11-15 03:50_

Very nice, thanks Micha!

---

_Label `bug` added by @charliermarsh on 2024-11-15 03:51_

---

_Merged by @charliermarsh on 2024-11-15 04:03_

---

_Closed by @charliermarsh on 2024-11-15 04:03_

---

_Branch deleted on 2024-11-15 04:03_

---
