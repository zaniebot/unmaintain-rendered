```yaml
number: 16141
title: Preserve comments on version bump
type: pull_request
state: merged
author: assadyousuf
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/16133-version-bump-preserve-comments
created_at: 2025-10-06T18:45:11Z
updated_at: 2025-10-09T16:07:01Z
url: https://github.com/astral-sh/uv/pull/16141
synced_at: 2026-01-12T16:12:07Z
```

# Preserve comments on version bump

---

_@assadyousuf_

## Summary
Fixes [1633](https://github.com/astral-sh/uv/issues/16133). Preserves comments preceding "version = ..." line when uv version --bump is ran

## Test Plan
Added IT test

---

_Review comment by @konstin on `crates/uv-workspace/src/pyproject_mut.rs`:1157 on 2025-10-07 09:04_

Can we change the whole decor at once with `*formatted.decor_mut() = `?

---

_Review comment by @konstin on `crates/uv/tests/it/version.rs`:314 on 2025-10-07 09:05_

This test case should use a more minimal `pyproject.toml`, and the comments should be different, so we see if e.g. the order changed.

---

_Review comment by @konstin on `crates/uv/tests/it/version.rs`:305 on 2025-10-07 09:05_

Can you add an end-of-line comment after the version too?

---

_@konstin reviewed on 2025-10-07 09:07_

Thanks, I left some comments

---

_Renamed from "Fix: Preserve comments on version bump" to "Preserve comments on version bump" by @konstin on 2025-10-07 09:07_

---

_Label `bug` added by @konstin on 2025-10-07 09:08_

---

_@konstin reviewed on 2025-10-08 06:23_

---

_Review comment by @konstin on `crates/uv/tests/it/version.rs`:292 on 2025-10-08 06:23_

```suggestion
/// Preserve comments immediately preceding the version when bumping
```

---

_@konstin approved on 2025-10-08 06:24_

---

_Review requested from @konstin by @assadyousuf on 2025-10-09 15:47_

---

_Comment by @assadyousuf on 2025-10-09 15:47_

@konstin Anyway you can re-run the CI pipeline again?

---

_Comment by @konstin on 2025-10-09 15:49_

Sorry I missed that, just restarted. There's a partial GitHub currently, so maybe we have to wait until tomorrow.

---

_Merged by @konstin on 2025-10-09 16:07_

---

_Closed by @konstin on 2025-10-09 16:07_

---
