```yaml
number: 14867
title: "uv_build: Allow non-standard entrypoint names"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - build-backend
assignees: []
merged: true
base: main
head: konsti/allow-non-compliant-entrypoints
created_at: 2025-07-24T09:26:15Z
updated_at: 2025-07-24T12:12:37Z
url: https://github.com/astral-sh/uv/pull/14867
synced_at: 2026-01-10T06:53:02Z
```

# uv_build: Allow non-standard entrypoint names

---

_Pull request opened by @konstin on 2025-07-24 09:26_

It seems that non-standard entrypoints are still widely used, downgrading the error to a tracing warning.

Fixes #14442

---

_Label `enhancement` added by @konstin on 2025-07-24 09:26_

---

_Label `build-backend` added by @konstin on 2025-07-24 09:26_

---

_@edmorley reviewed on 2025-07-24 10:29_

---

_Review comment by @edmorley on `crates/uv-build-backend/src/metadata.rs`:625 on 2025-07-24 10:29_

```suggestion
                    dashes; non-compliant name: `{name}`"
```

---

_@zanieb approved on 2025-07-24 11:53_

---

_Merged by @konstin on 2025-07-24 12:12_

---

_Closed by @konstin on 2025-07-24 12:12_

---

_Branch deleted on 2025-07-24 12:12_

---
