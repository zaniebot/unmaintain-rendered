```yaml
number: 13712
title: Support finer-grained control when upgrading Python versions
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
assignees: []
merged: true
base: feature/transparent-python-upgrades
head: jtfm/python-upgrade-expanded-symlink-name
created_at: 2025-05-28T22:56:21Z
updated_at: 2025-06-10T17:24:28Z
url: https://github.com/astral-sh/uv/pull/13712
synced_at: 2026-01-12T16:10:49Z
```

# Support finer-grained control when upgrading Python versions

---

_@jtfmumm_

Previously a symlink directory would be named something like `python3.10`. This did not account for information like the architecture or variant. This PR supports finer-grained control when upgrading Python versions. Transparent upgrades can be tied to more than just the minor version (e.g., the architecture). Accordingly, symlink directories naming now follows the existing Python build standalone directory format (excluding patch and prerelease). For example: `cpython-3.10-macos-aarch64-none`.

This PR also adds a new `PythonInstallationMinorVersionKey` type that wraps a `PythonInstallationKey` but excludes the patch and prerelease in operations. This provides a more fine-grained key (and centralized code) for determining the current highest installation for a minor key. It also provides a more natural and type-based approach to naming symlink directories.

The `DirectorySymlink` constructor has also been refactored accept the complete `PythonInstallationKey` instead of individual components.

Depends on #13312. 


---

_Label `enhancement` added by @jtfmumm on 2025-05-28 22:56_

---

_Review requested from @konstin by @jtfmumm on 2025-05-28 22:56_

---

_Review requested from @zanieb by @jtfmumm on 2025-05-28 22:56_

---

_@jtfmumm reviewed on 2025-05-28 22:56_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/common/mod.rs`:558 on 2025-05-28 22:56_

This is no longer needed for existing tests.

---

_Renamed from "Use most of key in symlink directory name" to "Symlink directory name utilizes most of Python installation key (excluding patch and prerelease)" by @jtfmumm on 2025-05-28 23:04_

---

_Assigned to @zanieb by @zanieb on 2025-05-29 14:46_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:517 on 2025-05-29 14:48_

Can you explain why you took this approach instead of just making `patch` an `Option` on the `PythonInstallationKey`?

---

_@zanieb reviewed on 2025-05-29 14:48_

---

_Review comment by @jtfmumm on `crates/uv-python/src/installation.rs`:517 on 2025-05-29 14:59_

(I updated the PR description while you were commenting but I'll add some more context). 

First, I wanted a representation of the key that corresponds to Python upgrades. Making patch an `Option` would mean that you don't know without checking if a key is a "minor version key". This is useful for example when building a map and searching for the highest installation for each key. It also provides a natural place to centralize code related to these checks. Finally, it allowed me to replace the more ad hoc symlink directory naming approach with a more type-based approach. You could probably do these things without this type but the intent would be less clear and I think it would be more prone to error.

---

_@jtfmumm reviewed on 2025-05-29 14:59_

---

_Renamed from "Symlink directory name utilizes most of Python installation key (excluding patch and prerelease)" to "Support finer-grained control when upgrading Python versions" by @jtfmumm on 2025-05-29 15:02_

---

_@konstin reviewed on 2025-05-30 08:26_

---

_Review comment by @konstin on `crates/uv-python/src/installation.rs`:517 on 2025-05-30 08:26_

Can we do something like parameterizing the python installation key over the type of version that's inside of it or having wrapper types around a base key type? I find it strange that `PythonInstallationMinorVersionKey` carries a `patch` version when cast from PythonInstallationKey.

---

_@konstin reviewed on 2025-05-30 11:19_

---

_Review comment by @konstin on `crates/uv-python/src/installation.rs`:517 on 2025-05-30 11:19_

This type works for me if we frame it as a view type onto `PythonInstallationKey` without dedicated constructor.

---

_@jtfmumm reviewed on 2025-05-30 11:22_

---

_Review comment by @jtfmumm on `crates/uv-python/src/installation.rs`:517 on 2025-05-30 11:22_

Yeah that's the idea. I've removed the constructor

---

_Comment by @codspeed-hq[bot] on 2025-05-30 12:09_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/jtfm%2Fpython-upgrade-expanded-symlink-name)

### Merging #13712 will **improve performances by 17.89%**

<sub>Comparing <code>jtfm/python-upgrade-expanded-symlink-name</code> (d5422a6) with <code>main</code> (f20a25f)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 112 ns | 95 ns | +17.89% |


---

_Comment by @zanieb on 2025-05-30 12:38_

Sharing that false positive benchmark in https://discord.com/channels/1065233827569598464/1377989469579509841

---

_Comment by @konstin on 2025-06-04 17:03_

What happens if you let's do a sequence of operation on a mac alternating with x86_64 python and aarch64 Python? Do we ensure that you never switch from one to the other?

---

_Comment by @jtfmumm on 2025-06-05 13:35_

> What happens if you let's do a sequence of operation on a mac alternating with x86_64 python and aarch64 Python? Do we ensure that you never switch from one to the other?

The symlink directory now includes the arch and will always point to the latest patch version for that arch. A virtual environment will link to the symlink directory for the arch it was created with. If a virtual environment is for x86_64, it will only upgrade when that arch is upgraded (or a higher patch for that arch is installed).

---

_Merged by @jtfmumm on 2025-06-10 17:22_

---

_Closed by @jtfmumm on 2025-06-10 17:22_

---

_Branch deleted on 2025-06-10 17:22_

---
