```yaml
number: 15038
title: Preserve lowered extra build dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/lower
created_at: 2025-08-03T02:07:54Z
updated_at: 2025-08-04T21:42:13Z
url: https://github.com/astral-sh/uv/pull/15038
synced_at: 2026-01-10T06:44:33Z
```

# Preserve lowered extra build dependencies

---

_Pull request opened by @charliermarsh on 2025-08-03 02:07_

## Summary

I should've noticed this during review -- my bad -- but it looks like after lowering, we're converting back to `uv_pep508::Requirement`. This is mostly okay, but it's lossy for some lowerings. For example, we lose index pinning. With this PR, we now preserve the lowered types (`Requirement`).

Closes https://github.com/astral-sh/uv/issues/15037.


---

_Label `bug` added by @charliermarsh on 2025-08-03 02:07_

---

_Marked ready for review by @charliermarsh on 2025-08-03 02:14_

---

_@charliermarsh reviewed on 2025-08-03 02:15_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/build_requires.rs`:212 on 2025-08-03 02:15_

This is a wrapper type, mostly to avoid circular dependencies. We want `ExtraBuildRequires` to be usable in the build dispatch traits, but those can't depend on `uv-distribution` (and yet we want the lowering logic to live here). This type doesn't really appear anywhere, except for the specific callsites that do the lowering -- they then call `.into_inner` and most things take `ExtraBuildRequires.

(We also use this pattern for at least one other type, `LoweredRequirement`.)

---

_@charliermarsh reviewed on 2025-08-03 02:16_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/compile.rs`:486 on 2025-08-03 02:16_

I think these actually _aren't_ lowered, unless I'm misunderstanding. This is distinctly different from scripts (which now use `from_lowered`), where we lower the dependencies ahead of time.

---

_Review requested from @zanieb by @charliermarsh on 2025-08-03 02:17_

---

_@zanieb approved on 2025-08-04 21:24_

---

_Merged by @charliermarsh on 2025-08-04 21:42_

---

_Closed by @charliermarsh on 2025-08-04 21:42_

---

_Branch deleted on 2025-08-04 21:42_

---
