```yaml
number: 3862
title: Rename source tree to dynamic requirements files
type: pull_request
state: closed
author: konstin
labels:
  - internal
assignees: []
draft: true
base: main
head: konsti/dynamic-requirements-files
created_at: 2024-05-27T10:44:05Z
updated_at: 2024-06-01T20:05:50Z
url: https://github.com/astral-sh/uv/pull/3862
synced_at: 2026-01-12T16:05:54Z
```

# Rename source tree to dynamic requirements files

---

_@konstin_

The function of `source_trees` got me confused a good bit since the term sounds like we're installing the package, not just reading requirements from it. I'm trying to make this clearer by renaming them to `dynamic_requirements_files`.

No functional changes, purely a renaming plus docs change.

---

_Label `internal` added by @konstin on 2024-05-27 10:44_

---

_Comment by @codspeed-hq[bot] on 2024-05-27 10:49_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/dynamic-requirements-files)

### Merging #3862 will **degrade performances by 6.1%**

<sub>Comparing <code>konsti/dynamic-requirements-files</code> (7058492) with <code>main</code> (65b17f6)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/konsti/dynamic-requirements-files)._

### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/dynamic-requirements-files` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 897.5 ns | 955.8 ns | -6.1% |


---

_Renamed from "Rename `source_trees` to `dynamic_requirements_files`" to "Rename `source_trees` to `dynamic_requirements_directories`" by @konstin on 2024-05-27 15:01_

---

_Converted to draft by @konstin on 2024-05-27 15:02_

---

_Renamed from "Rename `source_trees` to `dynamic_requirements_directories`" to "Rename source tree to dynamic requirements files" by @konstin on 2024-05-27 15:04_

---

_Comment by @charliermarsh on 2024-06-01 20:05_

Let's close for now since these got unified a bit.

---

_Closed by @charliermarsh on 2024-06-01 20:05_

---
