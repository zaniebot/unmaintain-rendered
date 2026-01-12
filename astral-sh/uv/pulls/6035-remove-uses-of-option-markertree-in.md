```yaml
number: 6035
title: "Remove uses of `Option<MarkerTree>` in `ResolutionGraph`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - preview
  - lock
assignees: []
merged: true
base: main
head: ibraheem/option-marker-fix
created_at: 2024-08-12T13:53:33Z
updated_at: 2024-08-12T14:31:09Z
url: https://github.com/astral-sh/uv/pull/6035
synced_at: 2026-01-12T16:07:10Z
```

# Remove uses of `Option<MarkerTree>` in `ResolutionGraph`

---

_@ibraheemdev_

## Summary

Missed this one in https://github.com/astral-sh/uv/pull/5978.

Resolves https://github.com/astral-sh/uv/issues/5902.

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-08-12 13:59_

---

_Label `lock` added by @ibraheemdev on 2024-08-12 13:59_

---

_Label `preview` added by @ibraheemdev on 2024-08-12 13:59_

---

_@ibraheemdev reviewed on 2024-08-12 14:07_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolution/display.rs`:469 on 2024-08-12 14:07_

This feels a little odd..

---

_@charliermarsh approved on 2024-08-12 14:08_

---

_@charliermarsh reviewed on 2024-08-12 14:08_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution/display.rs`:469 on 2024-08-12 14:08_

You could also just OR them and simplify? But this seems simpler.

---

_Merged by @ibraheemdev on 2024-08-12 14:31_

---

_Closed by @ibraheemdev on 2024-08-12 14:31_

---

_Branch deleted on 2024-08-12 14:31_

---
