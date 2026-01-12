```yaml
number: 3904
title: "Add support for `tool.uv` into distribution building"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/move-requirement-to-pypi-types
created_at: 2024-05-29T14:37:56Z
updated_at: 2024-05-31T13:44:22Z
url: https://github.com/astral-sh/uv/pull/3904
synced_at: 2026-01-12T16:05:55Z
```

# Add support for `tool.uv` into distribution building

---

_@konstin_

With the change, we remove the special casing of workspace dependencies and resolve `tool.uv` for all git and directory distributions. This gives us support for non-editable workspace dependencies and path dependencies in other workspaces. It removes a lot of special casing around workspaces. These changes are the groundwork for supporting `tool.uv` with dynamic metadata.

The basis for this change is moving `Requirement` from `distribution-types` to `pypi-types` and the lowering logic from `uv-requirements` to `uv-distribution`. This changes should be split out in separate PRs.

I've included an example workspace `albatross-root-workspace2` where `bird-feeder` depends on `a` from another workspace `ab`. There's a bunch of failing tests and regressed error messages that still need fixing. It does fix the audited package count for the workspace tests.

---

_Comment by @charliermarsh on 2024-05-29 19:57_

Can `ArchiveMetadata` just be removed entirely? Do we need both `ArchiveMetadata` and `ArchiveMetadataLowered`?

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/lib.rs`:61 on 2024-05-29 19:58_

Could we just call this `Metadata` or something, differentiated from `Metadata23` from the spec? Or use `pypi_types::Metadata23` everywhere?

---

_@charliermarsh reviewed on 2024-05-29 19:58_

---

_@konstin reviewed on 2024-05-29 20:21_

---

_Review comment by @konstin on `crates/uv-distribution/src/lib.rs`:61 on 2024-05-29 20:21_

Absolutely these names are only for me to be easier to work with and suppose to change before merging, ideally we'd even remove some type

---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:331 on 2024-05-29 20:30_

These were intended as unit tests for the lowering, but the api that i used to the reading went away so now we need to hook this somewhere different

---

_@konstin reviewed on 2024-05-29 20:30_

---

_@charliermarsh reviewed on 2024-05-29 21:26_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:1160 on 2024-05-29 21:26_

@konstin - Do you think we should be caching the lowered or non-lowered metadata here? What are the implications of caching the non-lowered metadata, then lowering it after?

---

_Comment by @charliermarsh on 2024-05-29 23:33_

@konstin - This is passing tests on Linux and macOS now, one Windows-specific failure.

---

_@charliermarsh reviewed on 2024-05-29 23:39_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/lib.rs`:71 on 2024-05-29 23:39_

Kind of wish none of this were async. Wondering if it shouldn't be given that it's all fs operations.

---

_@charliermarsh approved on 2024-05-29 23:41_

---

_Marked ready for review by @charliermarsh on 2024-05-29 23:41_

---

_Comment by @codspeed-hq[bot] on 2024-05-29 23:47_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/move-requirement-to-pypi-types)

### Merging #3904 will **degrade performances by 5.28%**

<sub>Comparing <code>konsti/move-requirement-to-pypi-types</code> (f2f217f) with <code>main</code> (09f5548)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/konsti/move-requirement-to-pypi-types)._

### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/move-requirement-to-pypi-types` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_jupyter` | 83.5 ms | 88.1 ms | -5.28% |


---

_Comment by @charliermarsh on 2024-05-30 20:30_

The member globbing isn't working properly on Windows.

---

_Comment by @charliermarsh on 2024-05-30 21:43_

I think I fixed it but need to rebase.

---

_Label `internal` added by @charliermarsh on 2024-05-31 02:22_

---

_Comment by @charliermarsh on 2024-05-31 02:26_

@konstin - I got this into a mergable state, but I removed `ab`, `albatross-root-workspace2`, and the commented-out tests in `specification.rs`. Can you restore any of those that are relevant in a separate PR?



---

_Merged by @charliermarsh on 2024-05-31 02:42_

---

_Closed by @charliermarsh on 2024-05-31 02:42_

---

_Branch deleted on 2024-05-31 02:42_

---

_Review comment by @konstin on `crates/uv-distribution/src/lib.rs`:71 on 2024-05-31 11:24_

Like i said in the initial PR, those are only async for consistency, we can totally make them sync

---

_@konstin reviewed on 2024-05-31 11:24_

---

_Comment by @konstin on 2024-05-31 13:44_

Restored the commented out tests, wrote a follow-up in https://github.com/astral-sh/uv/issues/3943

---

_Comment by @charliermarsh on 2024-05-31 13:44_

Thanks!

---
