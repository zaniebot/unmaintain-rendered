```yaml
number: 2381
title: Move architecture and operating system probing to Python
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/get-arch-and-os-from-python
created_at: 2024-03-12T12:47:04Z
updated_at: 2024-03-13T11:51:16Z
url: https://github.com/astral-sh/uv/pull/2381
synced_at: 2026-01-12T16:05:00Z
```

# Move architecture and operating system probing to Python

---

_@konstin_

The architecture of uv does not necessarily match that of the python interpreter (#2326). In cross compiling/testing scenarios the operating system can also mismatch. To solve this, we move arch and os detection to python, vendoring the relevant pypa/packaging code, preventing mismatches between what the python interpreter was compiled for and what uv was compiled for. 

To make the scripts more manageable, they are now a directory in a tempdir and we run them with `python -m` . I've simplified the pypa/packaging code since we're still building the tags in rust. A `Platform` is now instantiated by querying the python interpreter for its platform. The pypa/packaging files are copied verbatim for easier updates except a `lru_cache()` python 3.7 backport.

Error handling is done by a `"result": "success|error"` field that allow passing error details to rust:

```console
$ uv venv --no-cache
  × Can't use Python at `/home/konsti/projects/uv/.venv/bin/python3`
  ╰─▶ Unknown operation system `linux`
```

I've used the [maturin sysconfig collection](https://github.com/PyO3/maturin/tree/855f6d2cb1fb8fb43c2bb9e500ab0e5e84bd3140/sysconfig) as reference. I'm unsure how to test these changes across the wide variety of platforms.

Fixes #2326

---

_Marked ready for review by @konstin on 2024-03-12 13:40_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-12 14:18_

---

_@zanieb reviewed on 2024-03-12 14:54_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/interpreter.rs`:364 on 2024-03-12 14:54_

Not to be annoying here, but are we going to say we're okay with temporary directories outside of scopes we control? I understand the system cleanup argument but perhaps we should be garbage collecting them ourselves from a dedicated cache directory?

---

_@charliermarsh reviewed on 2024-03-12 14:59_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:449 on 2024-03-12 14:59_

I'm 100% serious, should we consider concatenating these into a single Python file for simplicity?

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:449 on 2024-03-12 15:28_

This was my first idea, i like the current one for manageable file sizes and because we can easily update from pypa/packaging upstream 

---

_@konstin reviewed on 2024-03-12 15:28_

---

_@konstin reviewed on 2024-03-12 15:31_

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:364 on 2024-03-12 15:31_

Unless there is a specific problem with the system directories, i believe we should follow system conventions and user's settings over rolling our own. For our own garbage collection, we'd also have to implement our own synchronization when there are multiple uv instances running in parallel.

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:364 on 2024-03-12 16:06_

I'd say let's just put it in the cache for consistency. It shouldn't be too hard here, right?

---

_@charliermarsh reviewed on 2024-03-12 16:06_

---

_@charliermarsh reviewed on 2024-03-12 16:07_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:449 on 2024-03-12 16:07_

What about a processing script that merges them?

---

_@charliermarsh reviewed on 2024-03-12 16:10_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:449 on 2024-03-12 16:10_

I guess this is PyInstaller?

---

_@konstin reviewed on 2024-03-12 16:14_

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:449 on 2024-03-12 16:14_

If we can script it i'm in favor. Maybe https://docs.python.org/3/library/zipimport.html is for that case? I haven't used it myself but i know people use it for bundling use cases.

---

_@zanieb reviewed on 2024-03-12 16:56_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/interpreter.rs`:364 on 2024-03-12 16:56_

What synchronization would we need? I would just delete old temporary directories (i.e. 24 hours) similar to how `/tmp` can be cleaned at arbitrary times.

---

_@konstin reviewed on 2024-03-12 16:59_

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:364 on 2024-03-12 16:59_

We can do that. What does it give us over the system temp?

---

_@charliermarsh reviewed on 2024-03-12 20:21_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/interpreter.rs`:364 on 2024-03-12 20:21_

I also don't follow the motivation yet.

---

_@zanieb reviewed on 2024-03-12 21:06_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/interpreter.rs`:364 on 2024-03-12 21:06_

I've heard of problems being caused by the system cleaning up temporary directories at arbitrary times which can break your program. Notably though we've already had problems with the system temporary directory (as discussed in the copy-on-write issue) and it seems safer to just use a directory we control. I'm totally fine with us deciding we want to use the system temporary directory, but I want us to be consistent if feasible or be clear about _when_ we should use one vs another. 

@charliermarsh previously @konstin said that using the system temporary directory was preferred for automated cleanup if we fail to clean up the directory i.e. if we crash.

---

_@charliermarsh reviewed on 2024-03-13 00:15_

---

_Review comment by @charliermarsh on `crates/uv-resolver/tests/resolver.rs`:123 on 2024-03-13 00:15_

What's going on here?

---

_@charliermarsh reviewed on 2024-03-13 00:20_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/python/get_interpreter_info.py`:477 on 2024-03-13 00:20_

I think this is a typo, should be `"name" :operating_system`

---

_@charliermarsh approved on 2024-03-13 04:19_

Looks good to me! Nice work. I just made a few small tweaks and fixed what I think is one bug in the BSD path.

---

_Renamed from " Move arch and os probing to python" to "Move architecture and operating system probing to Python" by @charliermarsh on 2024-03-13 04:21_

---

_Label `bug` added by @charliermarsh on 2024-03-13 04:21_

---

_Comment by @charliermarsh on 2024-03-13 04:39_

@konstin - I tried to merge this but `windows_shims` was failing after merging in `main`, I have no idea why.

---

_@konstin reviewed on 2024-03-13 09:58_

---

_Review comment by @konstin on `crates/uv-interpreter/src/interpreter.rs`:364 on 2024-03-13 09:58_

> I've heard of problems being caused by the system cleaning up temporary directories at arbitrary times which can break your program. 

If we can get a reproduction for that i'm all for switching to never using system temp again.

> Notably though we've already had problems with the system temporary directory (as discussed in the copy-on-write issue) and it seems safer to just use a directory we control. 

That is a very specific problem: Operating system don't offer atomic write operations, so e.g. two installer threads writing the same file could produce something broken/crash. Renames are (assumed to be) atomic and replace the existing file. Renames don't work across device boundaries (it would need a copy), so we need to create a named tempfile/tempdir in the same location as the final file, write to it and persist it. Some people e.g. put a ramdisk as their `/tmp` or the target fs is part of an overlayfs while tmp isn't, or the project lies on a different partition or drive than the system.

---

_@konstin reviewed on 2024-03-13 10:04_

---

_Review comment by @konstin on `crates/uv-resolver/tests/resolver.rs`:123 on 2024-03-13 10:04_

The fake interpreter needs to know it's platform to compute the tags we need for resolution. Previously, we could ask in rust, now we need an actual python interpreter. Assuming the dev has any python 3 with the right os + arch installed seemed like an ok assumption here, but i can pull in our bootstrapped python finding for a correct works-independent-of-outside-project-environment solution.

---

_Comment by @konstin on 2024-03-13 11:38_

It was the usual suspect again: bat files don't support UNC paths so the shim was skipped.

---

_Merged by @charliermarsh on 2024-03-13 11:51_

---

_Closed by @charliermarsh on 2024-03-13 11:51_

---

_Branch deleted on 2024-03-13 11:51_

---
