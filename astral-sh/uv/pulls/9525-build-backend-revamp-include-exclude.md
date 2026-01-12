```yaml
number: 9525
title: " Build backend: Revamp include/exclude"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-warn-extra
created_at: 2024-11-29T15:43:27Z
updated_at: 2024-12-03T14:14:42Z
url: https://github.com/astral-sh/uv/pull/9525
synced_at: 2026-01-12T16:08:50Z
```

#  Build backend: Revamp include/exclude

---

_@konstin_

When building the source distribution, we always need to include `pyproject.toml` and the module, when building the wheel, we always include the module but nothing else at top level. Since we only allow a single module per wheel, that means that there are no specific wheel includes. This means we have source includes, source excludes, wheel excludes, but no wheel includes: This is defined by the module root, plus the metadata files and data directories separately.

Extra source dist includes are currently unused (they can't end up in the wheel currently), but it makes sense to model them here, they will be needed for any sort of procedural build step.

This results in the following fields being relevant for inclusions and exclusion:

* `pyproject.toml` (always included in the source dist)
* project.readme: PEP 621
* project.license-files: PEP 639
* module_root: `Path`
* source_include: `Vec<Glob>`
* source_exclude: `Vec<Glob>`
* wheel_exclude: `Vec<Glob>`
* data: `Map<KnownDataName, Path>`

An opinionated choice is that that wheel excludes always contain the source excludes: Otherwise you could have a path A in the source tree that gets included when building the wheel directly from the source tree, but not when going through the source dist as intermediary, because A is in source excludes, but not in the wheel excludes. This has been a source of errors previously.

In the process, I fixed a bug where we would skip directories and only include the files and were missing license due to absolute globs.

---

_Label `preview` added by @konstin on 2024-11-29 15:43_

---

_Review requested from @BurntSushi by @konstin on 2024-11-29 15:48_

---

_Merged by @konstin on 2024-12-01 11:32_

---

_Closed by @konstin on 2024-12-01 11:32_

---

_Branch deleted on 2024-12-01 11:32_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/metadata.rs`:734 on 2024-12-03 14:14_

This was a helpful comment, thank you!

---

_@BurntSushi reviewed on 2024-12-03 14:14_

---
