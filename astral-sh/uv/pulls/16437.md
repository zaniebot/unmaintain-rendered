```yaml
number: 16437
title: "ci: re-enable zizmor"
type: pull_request
state: merged
author: woodruffw
labels: []
assignees: []
merged: true
base: main
head: ww/reenable-zizmor
created_at: 2025-10-24T15:01:04Z
updated_at: 2025-10-24T15:34:35Z
url: https://github.com/astral-sh/uv/pull/16437
synced_at: 2026-01-10T06:36:16Z
```

# ci: re-enable zizmor

---

_Pull request opened by @woodruffw on 2025-10-24 15:01_

## Summary

The latest version should fix our rate-limiting issues.

## Test Plan

See what happens in CI.

---

_Comment by @zanieb on 2025-10-24 15:08_

Did the version get bumped?

---

_Comment by @woodruffw on 2025-10-24 15:13_

> Did the version get bumped?

Yeah, the action implicitly uses the latest version. We could explicitly set it with `version` instead:

https://github.com/zizmorcore/zizmor-action#version

Log line for the bumped version: https://github.com/astral-sh/uv/actions/runs/18783696637/job/53596315502?pr=16437#step:3:82

---

_Comment by @zanieb on 2025-10-24 15:31_

> Yeah, the action implicitly uses the latest version. We could explicitly set it with version instead:
>
> https://github.com/zizmorcore/zizmor-action#version

Oh!

That's sort of dubious from a security perspective here :) but alrighty



---

_@zanieb approved on 2025-10-24 15:31_

---

_Merged by @zanieb on 2025-10-24 15:31_

---

_Closed by @zanieb on 2025-10-24 15:31_

---

_Branch deleted on 2025-10-24 15:31_

---

_Comment by @woodruffw on 2025-10-24 15:34_

> That's sort of dubious from a security perspective here :) but alrighty

Yeah, agreed...this is why the workflow intentionally runs with dropped permissions (and doesn't do any cache reads or writes, etc.), but it's something I've thought about changing.


---
