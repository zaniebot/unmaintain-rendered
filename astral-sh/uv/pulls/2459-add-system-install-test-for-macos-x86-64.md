```yaml
number: 2459
title: Add system install test for macOS x86_64
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/macos-x64
created_at: 2024-03-14T14:48:12Z
updated_at: 2024-03-14T17:26:47Z
url: https://github.com/astral-sh/uv/pull/2459
synced_at: 2026-01-12T16:05:03Z
```

# Add system install test for macOS x86_64

---

_@zanieb_

Adds binary builds for x86_64 macOS and a corresponding test

---

_@charliermarsh approved on 2024-03-14 15:10_

---

_@AlexWaygood reviewed on 2024-03-14 15:26_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yml`:395 on 2024-03-14 15:26_

It looks like the failing job here ran on macOS 12.7.3: https://github.com/AlexWaygood/typeshed-stats/actions/runs/8279567587/job/22654258397?pr=206

---

_@AlexWaygood reviewed on 2024-03-14 15:27_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yml`:395 on 2024-03-14 15:27_

I just use "macos-latest" in that job, but it looks like that does not in fact resolve to the latest macos version

---

_@zanieb reviewed on 2024-03-14 15:28_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:395 on 2024-03-14 15:28_

Oh we should be using `macos-latest` here 🤦 

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:395 on 2024-03-14 15:28_

(I'm a dumbo)

---

_@zanieb reviewed on 2024-03-14 15:28_

---

_@zanieb reviewed on 2024-03-14 15:29_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:395 on 2024-03-14 15:29_

`macos-14` is the ARM machine

---

_Marked ready for review by @zanieb on 2024-03-14 16:11_

---

_Merged by @zanieb on 2024-03-14 17:26_

---

_Closed by @zanieb on 2024-03-14 17:26_

---

_Branch deleted on 2024-03-14 17:26_

---
