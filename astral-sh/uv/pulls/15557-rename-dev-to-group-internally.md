```yaml
number: 15557
title: "Rename `Dev` to `Group` internally"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/dev-to-group
created_at: 2025-08-27T18:12:48Z
updated_at: 2025-08-27T18:35:44Z
url: https://github.com/astral-sh/uv/pull/15557
synced_at: 2026-01-10T06:44:33Z
```

# Rename `Dev` to `Group` internally

---

_Pull request opened by @konstin on 2025-08-27 18:12_

The "dev" naming is a pre-PEP 735 artifact.

---

_Label `internal` added by @konstin on 2025-08-27 18:12_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:3251 on 2025-08-27 18:14_

Looks like something should change here

---

_@zanieb reviewed on 2025-08-27 18:14_

---

_@zanieb reviewed on 2025-08-27 18:14_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:3069 on 2025-08-27 18:14_

Here too

---

_@zanieb approved on 2025-08-27 18:15_

All for this. I imagine there are a lot of small changes to make too.

---

_Comment by @konstin on 2025-08-27 18:25_

Did a more thorough search to fix more cases (including the ones you noted).

I haven't changed "development dependency group" in the error message and comments to "dependency group" yet, maybe a good task for https://github.com/astral-sh/uv/issues/15406.

---

_Comment by @zanieb on 2025-08-27 18:34_

> haven't changed "development dependency group" in the error message and comments to "dependency group" yet, maybe a good task for https://github.com/astral-sh/uv/issues/15406.

I think we might want to keep that? I worry they're confusing otherwise. We can discuss that separately though.

---

_Merged by @konstin on 2025-08-27 18:35_

---

_Closed by @konstin on 2025-08-27 18:35_

---

_Branch deleted on 2025-08-27 18:35_

---
