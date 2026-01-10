```yaml
number: 13531
title: Support transparently upgradeable Python bin installations
type: pull_request
state: merged
author: jtfmumm
labels:
  - enhancement
assignees: []
merged: true
base: feature/transparent-python-upgrades
head: jtfm/python-upgrade-bin
created_at: 2025-05-19T08:55:36Z
updated_at: 2025-06-10T18:05:00Z
url: https://github.com/astral-sh/uv/pull/13531
synced_at: 2026-01-10T11:10:41Z
```

# Support transparently upgradeable Python bin installations

---

_Pull request opened by @jtfmumm on 2025-05-19 08:55_

For `uv python install --preview` and `uv python install <minor-version> --preview`, this installs transparently upgradeable executables into `bin`. On macOS/Linux, it uses a symlink pointing to a path containing a minor version directory symlink. On Windows, it uses a trampoline with a junction-containing path as its target.

When creating `bin` links, if the install is either a default install or a minor version install, we check if we can derive a `DirectorySymlink` from each installation. If we can, then
* on macOS/Linux, we use the `DirectorySymlink` to link the `bin` link to an executable path containing the symlink directory. 
* on Windows, we use the `DirectorySymlink` (which contains a junction) to create our trampoline.

On Windows, this also updates a couple of other things:
* When doing comparisons to determine if we have a bin link, if we have a trampoline, we fully resolve its target path using `dunce::canonicalize`.
* In the trampoline itself, if it's a Python trampoline, we check if we're in a virtual environment (looking for a `pyvenv.cfg` with a `home` key). If we are not in a virtual environment and the `PYTHONHOME` env var is not set, we set `PYTHONHOME` to the parent directory of the executable in order to ensure that `sys.path` is set with the correct values.

This supports both upgradeable `python{major}.{minor}` and freethreaded `python{major}.{minor}t` installs. `uv python upgrade 3.13` will upgrade the former while `uv python upgrade 3.13t` will upgrade the latter.

This depends on PR #13712. 


---

_@zanieb reviewed on 2025-05-19 13:38_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:644 on 2025-05-19 13:38_

Why must the path be absolute?

---

_Comment by @zanieb on 2025-05-19 13:40_

Can you add a bit more to the pull request summary about how this works? I could probably glean it from the implementation, but I'd like to understand how it's supposed to work before reading the implementation.

---

_@jtfmumm reviewed on 2025-05-19 13:56_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:644 on 2025-05-19 13:56_

This method is moved from `uv_fs` and this was an existing assumption.

---

_@jtfmumm reviewed on 2025-05-19 14:00_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:644 on 2025-05-19 14:00_

To be clear, it's also been updated to check on Windows for a trampoline, and if one is found, to use `dunce::canonicalize` to find the fully resolved path.

---

_@zanieb reviewed on 2025-05-19 14:06_

---

_Review comment by @zanieb on `crates/uv-python/src/interpreter.rs`:644 on 2025-05-19 14:06_

I don't understand why the existing assumption was there though, I think it's because otherwise it would be surprising behavior on Windows but you changed the Windows behavior here.

---

_Comment by @jtfmumm on 2025-05-19 14:10_

> Can you add a bit more to the pull request summary about how this works? I could probably glean it from the implementation, but I'd like to understand how it's supposed to work before reading the implementation.

I've added a couple of new paragraphs with more detail.

---

_Comment by @zanieb on 2025-05-19 14:21_

Thanks!

And just to clarify, I thought `~/.local/bin/python3.11 -m venv` wouldn't work on Unix. How was that resolved?

---

_Comment by @jtfmumm on 2025-05-19 14:32_

> Thanks!
> 
> And just to clarify, I thought `~/.local/bin/python3.11 -m venv` wouldn't work on Unix. How was that resolved?

I just wasn't certain it would work for every scenario, but when testing further I found there was no issue.

---

_Label `enhancement` added by @jtfmumm on 2025-05-20 19:25_

---

_Assigned to @zanieb by @zanieb on 2025-05-21 15:15_

---

_@jtfmumm reviewed on 2025-05-21 20:34_

---

_Review comment by @jtfmumm on `crates/uv-python/src/interpreter.rs`:644 on 2025-05-21 20:34_

This can still behave in the old way if the executable is not a trampoline. I can look into why this `debug_assert` was added in the first place, but I don't think my changes would have changed the invariant for the non-trampoline case.

---

_Comment by @codspeed-hq[bot] on 2025-05-26 13:09_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/jtfm%2Fpython-upgrade-bin)

### Merging #13531 will **not alter performance**

<sub>Comparing <code>jtfm/python-upgrade-bin</code> (89b1f78) with <code>main</code> (f20a25f)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:612 on 2025-06-04 15:10_

Can you add about what the `canonicalize` and the `unwrap_or` fallback do here?

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:96 on 2025-06-04 15:18_

no a code problem, for my own understanding: what's wrong with `sys.path` if we don't set `PYTHONHOME`?

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:161 on 2025-06-04 15:21_

We usually combine those by opening directly and having `Err(err) if err.kind() == io::ErrorKind::NotFound` in the error message, but here we may skip the `if pyvenv_path.exists()` since it's included in discarding open errors.

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:164 on 2025-06-04 15:22_

I think we don't trim in the other places where we parse `pyvenv.cfg`

---

_Review comment by @konstin on `crates/uv/tests/it/python_install.rs`:1138 on 2025-06-04 15:25_

```suggestion
        .unwrap_or_else(|| panic!("{} should be readable", path.display()))
```

---

_@konstin reviewed on 2025-06-04 15:27_

Left some minor comments

---

_@jtfmumm reviewed on 2025-06-08 23:22_

---

_Review comment by @jtfmumm on `crates/uv-python/src/managed.rs`:612 on 2025-06-08 23:22_

Added

---

_@jtfmumm reviewed on 2025-06-08 23:28_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:96 on 2025-06-08 23:28_

There were cases where running with the `bin` trampoline was setting the effective home directory to as I recall the `bin` directory. And so, for example, the wrong `site-packages` path was being set. 

---

_Renamed from "Support transparently upgradeable Python bin installations (under `--preview`)" to "Support transparently upgradeable Python bin installations" by @jtfmumm on 2025-06-09 18:03_

---

_@zanieb reviewed on 2025-06-10 12:28_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:167 on 2025-06-10 12:28_

Why do we need to search for this key? The presence of a `pyvenv.cfg` file alone seems sufficient?

---

_@zanieb reviewed on 2025-06-10 12:31_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:158 on 2025-06-10 12:31_

Why are we looking in these two directories? We usually just check in the environment root, which should always be the grandparent of a Python executable.

---

_@zanieb reviewed on 2025-06-10 12:31_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:167 on 2025-06-10 12:31_

(We have other checks for virtual environments and they do not read the file)

---

_@jtfmumm reviewed on 2025-06-10 12:37_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:167 on 2025-06-10 12:37_

I'm following PEP 405 here (same answer for your question about checking two directories):

```
This PEP proposes to add a new first step to this search. If a `pyvenv.cfg` file is found either adjacent to the Python executable or one directory above it (if the executable is a symlink, it is not dereferenced), this file is scanned for lines of the form `key = value`. If a `home` key is found, this signifies that the Python binary belongs to a virtual environment, and the value of the home key is the directory containing the Python executable used to create this virtual environment.
```

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:158 on 2025-06-10 12:38_

See response above

---

_@jtfmumm reviewed on 2025-06-10 12:38_

---

_@jtfmumm reviewed on 2025-06-10 12:41_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:167 on 2025-06-10 12:41_

But if we've found this is unnecessary I could update it

---

_@zanieb reviewed on 2025-06-10 12:54_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:167 on 2025-06-10 12:54_

Weird. We don't do that today. I'm not sure scanning for the `home` key is necessary. I also can't think of a situation where the `pyvenv.cfg` would be next to the executable. I would probably just note that we're not doing this more robust check.

---

_@zanieb reviewed on 2025-06-10 12:55_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:167 on 2025-06-10 12:55_

e.g.

https://github.com/astral-sh/uv/blob/bf96c60e3eb805ba9d66630e87cd639c75f81f0a/crates/uv-python/src/interpreter.rs#L948-L952

https://github.com/astral-sh/uv/blob/fb0811680054a8aea9d8babdbf6877c6e3da9afe/crates/uv-python/src/virtualenv.rs#L134

https://github.com/astral-sh/uv/blob/7310ea75da16e6b8c86bbc11c12d6c46a15b8f8e/crates/uv-python/src/discovery.rs#L829-L831

---

_@jtfmumm reviewed on 2025-06-10 13:06_

---

_Review comment by @jtfmumm on `crates/uv-trampoline/src/bounce.rs`:167 on 2025-06-10 13:06_

Ok, I'll update and comment

---

_Comment by @codspeed-hq[bot] on 2025-06-10 17:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/jtfm%2Fpython-upgrade-bin)

### Merging #13531 will **improve performances by 17.89%**

<sub>Comparing <code>jtfm/python-upgrade-bin</code> (89b1f78) with <code>main</code> (f20a25f)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 112 ns | 95 ns | +17.89% |


---

_Comment by @jtfmumm on 2025-06-10 18:04_

Merging into feature branch as part of the PR stack (as described in [this comment](https://github.com/astral-sh/uv/pull/13312#issuecomment-2960012218))

---

_Merged by @jtfmumm on 2025-06-10 18:04_

---

_Closed by @jtfmumm on 2025-06-10 18:04_

---

_Branch deleted on 2025-06-10 18:05_

---
