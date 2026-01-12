```yaml
number: 11122
title: Use explicit _GLibCVersion tuple in uv-python crate
type: pull_request
state: merged
author: cthoyt
labels: []
assignees: []
merged: true
base: main
head: explicit-glibcversion-tuples
created_at: 2025-01-30T23:30:21Z
updated_at: 2025-01-31T11:31:42Z
url: https://github.com/astral-sh/uv/pull/11122
synced_at: 2026-01-12T16:09:41Z
```

# Use explicit _GLibCVersion tuple in uv-python crate

---

_@cthoyt_

## Summary

I get the following type checking errors in the uv-python crate when running mypy:

```console
$ cd crates/uv-python
$ uvx --python 3.13 --with httpx --with types-setuptools --with chevron-blue mypy --ignore-missing-imports .

python/get_interpreter_info.py:549: error: Argument "version" to "_is_compatible" has incompatible type "tuple[int, int]"; expected "_GLibCVersion"  [arg-type]
Found 1 error in 1 file (checked 8 source files)
```

What this PR does:

1. Makes the type annotations more consistent in the uv-python crate in the code that parses the glibc version, addressing this warning. This mirrors the _MuslVersion named tuple in the sibling `_musllinux.py` file
2. Adds an explicit type annotation to the `_LEGACY_MANYLINUX_MAP` dictionary, which also used `tuple[int, int]` instead of `_GLibCVersion`

## Test Plan

Running the same command that got the error before is now all gucci!

```console
$ cd crates/uv-python
$ uvx --python 3.13 --with httpx --with types-setuptools --with chevron-blue mypy --ignore-missing-imports .

Success: no issues found in 8 source files
```


---

_Renamed from "Use explicit _GLibCVersion tuple" to "Use explicit _GLibCVersion tuple un uv-python crate" by @cthoyt on 2025-01-30 23:31_

---

_Renamed from "Use explicit _GLibCVersion tuple un uv-python crate" to "Use explicit _GLibCVersion tuple in uv-python crate" by @cthoyt on 2025-01-30 23:45_

---

_@konstin approved on 2025-01-31 10:51_

---

_Merged by @konstin on 2025-01-31 10:52_

---

_Closed by @konstin on 2025-01-31 10:52_

---

_Branch deleted on 2025-01-31 10:57_

---

_Comment by @cthoyt on 2025-01-31 11:31_

I upstreamed this in https://github.com/pypa/packaging/pull/868

---
