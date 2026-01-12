```yaml
number: 14156
title: Update cargo-dist
type: pull_request
state: merged
author: Gankra
labels:
  - internal
assignees: []
merged: true
base: main
head: gankra/bump-dist
created_at: 2025-06-20T15:02:46Z
updated_at: 2025-06-20T18:20:02Z
url: https://github.com/astral-sh/uv/pull/14156
synced_at: 2026-01-12T16:11:03Z
```

# Update cargo-dist

---

_@Gankra_

Also took the time to migrate to the external config format to normalize our projects for team comfort (`ty` *has* to use this format for its workspace structure).

---

_Label `internal` added by @Gankra on 2025-06-20 15:02_

---

_Comment by @Gankra on 2025-06-20 15:24_

Filed https://github.com/astral-sh/uv/issues/14158 for the flake

---

_@zanieb reviewed on 2025-06-20 16:58_

---

_Review comment by @zanieb on `.github/workflows/release.yml`:72 on 2025-06-20 16:58_

Why pre?

---

_@Gankra reviewed on 2025-06-20 18:03_

---

_Review comment by @Gankra on `.github/workflows/release.yml`:72 on 2025-06-20 18:03_

tldr stable vs prerelease is kinda random with cargo-dist -- this is just "the latest release" (discussed it with a few folks previously and they didn't really care about the optics since it's our own tool).

The way cargo-dist works I often need to release it a couple times to make it use itself when releasing itself (to dogfood/smoketest). So I release many prereleases and then a final release. In this case the first prerelease had the functionality `ty` needed and it was for a feature dist couldn't dogfood (recursive source tarballs), so we moved on without doing a "proper" release.

---

_Comment by @Gankra on 2025-06-20 18:06_

Specifically this update is to land the fix for https://github.com/astral-sh/uv/issues/6965 (https://github.com/astral-sh/cargo-dist/pull/38)

---

_@zanieb approved on 2025-06-20 18:16_

---

_Merged by @Gankra on 2025-06-20 18:20_

---

_Closed by @Gankra on 2025-06-20 18:20_

---

_Branch deleted on 2025-06-20 18:20_

---
