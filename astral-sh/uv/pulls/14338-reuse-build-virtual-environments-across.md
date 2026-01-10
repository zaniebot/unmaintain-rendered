```yaml
number: 14338
title: Reuse build (virtual) environments across resolution and installation
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/shared-builds
created_at: 2025-06-27T23:41:42Z
updated_at: 2025-07-01T17:15:48Z
url: https://github.com/astral-sh/uv/pull/14338
synced_at: 2026-01-10T06:53:01Z
```

# Reuse build (virtual) environments across resolution and installation

---

_Pull request opened by @charliermarsh on 2025-06-27 23:41_

## Summary

The basic idea here is that we can (should) reuse a build environment across resolution (`prepare_metadata_for_build_wheel`) and installation. This also happens to solve the build-PyTorch-from-source problem, since we use a consistent build environment between the invocations.

Since `SourceDistributionBuilder` is stateless, we instead store the builds on `BuildContext`, and we key them by various properties: the underlying interpreter, the configuration settings, etc. This just ensures that if we build the same package twice within a process, we don't accidentally reuse an incompatible build (virtual) environment. (Note that still drop build environments at the end of the command, and don't attempt to reuse them across processes.)

Closes #14269.


---

_Marked ready for review by @charliermarsh on 2025-06-27 23:46_

---

_Review requested from @zanieb by @charliermarsh on 2025-06-27 23:50_

---

_Review requested from @konstin by @charliermarsh on 2025-06-27 23:50_

---

_Label `enhancement` added by @charliermarsh on 2025-06-27 23:50_

---

_Converted to draft by @charliermarsh on 2025-06-27 23:51_

---

_Comment by @charliermarsh on 2025-06-27 23:51_

(Converting to draft while I review test failures.)

---

_Comment by @charliermarsh on 2025-06-28 00:40_

There's a deadlock somewhere related to `--no-binary` but having a lot of trouble figuring out where.

---

_Marked ready for review by @charliermarsh on 2025-06-28 01:23_

---

_Comment by @charliermarsh on 2025-06-28 01:23_

Okay, tests passing.

---

_Review comment by @konstin on `crates/uv-distribution/src/source/mod.rs`:2330 on 2025-07-01 11:13_

After we removed it, should we re-add in case we need it again? We could have `prepare_metadata_for_build_wheel`, then `get_requires_for_build_wheel` and then `build_wheel`, which would be three steps.

(I want to keep the removing over borrowing/sharing as it safeguards against accidental parallelism)

---

_@konstin reviewed on 2025-07-01 11:27_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:2330 on 2025-07-01 14:00_

Yeah this makes sense. (I didn't want to borrow for the same reason.)

---

_@charliermarsh reviewed on 2025-07-01 14:00_

---

_@charliermarsh reviewed on 2025-07-01 14:01_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:2330 on 2025-07-01 14:01_

Done.

---

_Review comment by @konstin on `crates/uv-distribution/src/source/mod.rs`:2335 on 2025-07-01 14:02_

nit: we can move the duplicated code below the `if let`.

---

_@konstin approved on 2025-07-01 14:02_

---

_@charliermarsh reviewed on 2025-07-01 14:04_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:2335 on 2025-07-01 14:04_

Which code?

---

_Merged by @charliermarsh on 2025-07-01 17:15_

---

_Closed by @charliermarsh on 2025-07-01 17:15_

---

_Branch deleted on 2025-07-01 17:15_

---
