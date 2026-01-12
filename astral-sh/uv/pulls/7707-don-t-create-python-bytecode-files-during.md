```yaml
number: 7707
title: "Don't create Python bytecode files during interpreter discovery"
type: pull_request
state: merged
author: akx
labels:
  - bug
assignees: []
merged: true
base: main
head: discovery-without-bytecode
created_at: 2024-09-26T12:38:44Z
updated_at: 2024-09-26T13:53:00Z
url: https://github.com/astral-sh/uv/pull/7707
synced_at: 2026-01-12T16:07:57Z
```

# Don't create Python bytecode files during interpreter discovery

---

_@akx_

## Summary

As @konstin noted, `python -I` ignores `PYTHONDONTWRITEBYTECODE`, so `--no-compile-bytecode` may end up with bytecode spilled onto the disk nevertheless.

This should fix that by way of adding [`-B`](https://docs.python.org/3.12/using/cmdline.html#cmdoption-B).

## Test Plan

None so far. I can add proper tests if required, if this is deemed a good idea :)

Closes #7696.

---

_@zanieb approved on 2024-09-26 12:40_

Seems reasonable to me if @konstin signs off.

---

_Assigned to @konstin by @zanieb on 2024-09-26 12:40_

---

_Comment by @konstin on 2024-09-26 13:48_

We have test coverage through the general ecosystem and python discovery tests, i don't think we need specific tests that we are not writing bytecode.

For performance considerations: We're caching interpreter discovery, so this code should run only once, so the lack for writing bytecode should not matter. If this should turn out to affect could start times of containers, we should extend `--compile` to also bytecode compile the standard library, if permissions allow.

---

_Label `bug` added by @konstin on 2024-09-26 13:49_

---

_Renamed from "Disallow writing Python bytecode when querying interpreter info" to "Don't create Python bytecode files during interpreter discovery" by @konstin on 2024-09-26 13:49_

---

_Merged by @konstin on 2024-09-26 13:52_

---

_Closed by @konstin on 2024-09-26 13:52_

---

_Branch deleted on 2024-09-26 13:53_

---
