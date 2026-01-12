```yaml
number: 2382
title: Add failing python-build-standalone alpine test
type: pull_request
state: closed
author: konstin
labels:
  - musl
assignees: []
draft: true
base: main
head: konsti/failing-alpine-test
created_at: 2024-03-12T13:20:14Z
updated_at: 2024-10-15T07:31:53Z
url: https://github.com/astral-sh/uv/pull/2382
synced_at: 2026-01-12T16:05:00Z
```

# Add failing python-build-standalone alpine test

---

_@konstin_

Currently, python-build-standalone is failing on alpine (musl) with the following error:

> OSError: Dynamic loading not supported

iirc this is something that gregory mentioned, but it means that as of now, we can't adopt bootstrapping python-build-standalone as default when we want to support alpine.

---

_Label `musl` added by @konstin on 2024-03-12 13:20_

---

_@zanieb reviewed on 2024-03-12 14:12_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:35 on 2024-03-12 14:12_

What is this madness formatting? :D

---

_@konstin reviewed on 2024-03-12 14:52_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:35 on 2024-03-12 14:52_

pycharm

---

_@zanieb reviewed on 2024-03-12 16:54_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:35 on 2024-03-12 16:54_

Pretter (VSCode) conflicts with this. Can you turn off formatting for YAML? I've been wondering why these change all the time :p

Maybe we should consider codifying our YAML formatting.

---

_@konstin reviewed on 2024-03-13 10:35_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:35 on 2024-03-13 10:35_

https://github.com/astral-sh/uv/pull/2406

---

_@konstin reviewed on 2024-03-13 10:36_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:35 on 2024-03-13 10:36_

(I'm not fixing this PR until the underlying bug it fixed)

---

_Comment by @zanieb on 2024-10-14 21:49_

@konstin what's the plan here?

---

_Comment by @konstin on 2024-10-15 07:31_

We need to fix musl in python-build-standalone, then we can use this branch as a test that the problem with static vs dynamic linking is indeed fixed.

---

_Closed by @konstin on 2024-10-15 07:31_

---
