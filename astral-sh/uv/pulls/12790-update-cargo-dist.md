```yaml
number: 12790
title: update cargo-dist
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/update-dist2
created_at: 2025-04-09T21:21:52Z
updated_at: 2025-04-10T13:42:43Z
url: https://github.com/astral-sh/uv/pull/12790
synced_at: 2026-01-10T11:10:40Z
```

# update cargo-dist

---

_Pull request opened by @Gankra on 2025-04-09 21:21_

Putting this up to confirm that it does what it should:

* undirty the release.yml by including action-commits in the config
* add `persist-credentials=false` hardening
* includes but does not use `[package.metadata.dist.binaries]` overrides (for #11786)

You can choose to not merge this right away if a prerelease is conceptually spooky to you, otherwise it should work fine.


---

_@konstin approved on 2025-04-10 08:28_

---

_Merged by @Gankra on 2025-04-10 13:42_

---

_Closed by @Gankra on 2025-04-10 13:42_

---

_Branch deleted on 2025-04-10 13:42_

---
