```yaml
number: 7675
title: "Add `uv build --no-build-logs` to silence the build backend logs"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - cli
assignees: []
merged: true
base: main
head: zb/build-no-output
created_at: 2024-09-24T21:37:00Z
updated_at: 2024-09-26T23:39:48Z
url: https://github.com/astral-sh/uv/pull/7675
synced_at: 2026-01-10T12:53:53Z
```

# Add `uv build --no-build-logs` to silence the build backend logs

---

_Pull request opened by @zanieb on 2024-09-24 21:37_

Extends https://github.com/astral-sh/uv/pull/7674

The build backend can be pretty verbose, it seems nice to be able to turn that off?

---

_Label `cli` added by @zanieb on 2024-09-24 21:37_

---

_Comment by @charliermarsh on 2024-09-25 12:45_

This necessary given #7674? I'm just wary of adding more command-line flags without strong motivation.

---

_Comment by @zanieb on 2024-09-25 13:24_

They're pretty distinct i.e. I want to be able to share the output of the command in the docs but the build backend output is a lot on a trivial project. Maybe this will be solved by #3957 ?

---

_Comment by @charliermarsh on 2024-09-25 13:43_

Can you just use a `...` or similar in the docs? I guess I just don't see this as very useful in practice.

---

_Converted to draft by @zanieb on 2024-09-25 13:53_

---

_Comment by @zanieb on 2024-09-25 17:06_

Humph... easy enough to revisit if we need it later.

---

_Closed by @zanieb on 2024-09-25 17:06_

---

_Comment by @charliermarsh on 2024-09-26 23:22_

Want to reopen? I somewhat prefer `--no-build-output` so that it doesn't require users to know what a "backend" is, but understand that `uv build --no-build-output` may be awkward.

---

_Reopened by @charliermarsh on 2024-09-26 23:22_

---

_Marked ready for review by @charliermarsh on 2024-09-26 23:22_

---

_@charliermarsh approved on 2024-09-26 23:22_

---

_Comment by @zanieb on 2024-09-26 23:27_

> Want to reopen? I somewhat prefer `--no-build-output` so that it doesn't require users to know what a "backend" is, but understand that `uv build --no-build-output` may be awkward.

I felt the same thing and went back and forth. I figured it'd be better to be explicit since we don't foresee it being used by beginners? But `--no-build-output` sort of makes sense still... hm.

---

_Renamed from "Add `uv build --no-backend-output` to silence the build backend logs" to "Add `uv build --no-build-logs` to silence the build backend logs" by @zanieb on 2024-09-26 23:33_

---

_Label `internal` added by @zanieb on 2024-09-26 23:37_

---

_Merged by @zanieb on 2024-09-26 23:39_

---

_Closed by @zanieb on 2024-09-26 23:39_

---

_Branch deleted on 2024-09-26 23:39_

---
