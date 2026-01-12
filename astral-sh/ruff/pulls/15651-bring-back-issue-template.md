```yaml
number: 15651
title: Bring back issue template
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - internal
assignees: []
merged: true
base: main
head: issue-template
created_at: 2025-01-21T16:05:50Z
updated_at: 2025-01-22T12:08:03Z
url: https://github.com/astral-sh/ruff/pull/15651
synced_at: 2026-01-12T15:55:52Z
```

# Bring back issue template

---

_@InSyncWithFoo_

## Summary

Resolves #15641.

## Test Plan

![](https://github.com/user-attachments/assets/d0d5cf7c-f415-4f3e-934b-6f4ac5cada00)



---

_Comment by @InSyncWithFoo on 2025-01-21 16:06_

The `description` field is not shown to the user but mandatory nevertheless.

---

_Comment by @MichaReiser on 2025-01-21 17:19_

Nice, thank you. @zanieb this looks good to me, wdyt? 

@InSyncWithFoo just to confirm. Clicking on "new issue" now goes directly to the editor without showing a selector?

---

_Label `internal` added by @MichaReiser on 2025-01-21 17:19_

---

_Comment by @InSyncWithFoo on 2025-01-21 17:42_

> Clicking on "new issue" now goes directly to the editor without showing a selector?

Yes, that's correct.

----

`actionlint` seems to be complain about something, just like [last time](https://github.com/astral-sh/ruff/pull/15163/commits/f0876328080df57f95f100e8acd6df7f528a621e). I don't know what that is (line endings?).

---

_Comment by @AlexWaygood on 2025-01-21 18:04_

> `actionlint` seems to be complain about something, just like [last time](https://github.com/astral-sh/ruff/pull/15163/commits/f0876328080df57f95f100e8acd6df7f528a621e). I don't know what that is (line endings?).

(it's actually prettier that was complaining, not actionlint. I don't know what it was complaining about, but line endings seems plausible.)

---

_Comment by @zanieb on 2025-01-21 18:13_

Should we align these with the uv templates? or just restore this first?

---

_Comment by @zanieb on 2025-01-21 18:13_

See

- https://github.com/astral-sh/uv/pull/10786

---

_Comment by @MichaReiser on 2025-01-21 21:19_

We should restore them first. Adopting something similar to uv requires some more effort

---

_@MichaReiser approved on 2025-01-22 07:48_

This looks great to me. Thanks for updating the template. Let's see how it goes. We can iterate on the template as we see fit. 

---

_Merged by @MichaReiser on 2025-01-22 07:48_

---

_Closed by @MichaReiser on 2025-01-22 07:48_

---

_Branch deleted on 2025-01-22 12:08_

---
