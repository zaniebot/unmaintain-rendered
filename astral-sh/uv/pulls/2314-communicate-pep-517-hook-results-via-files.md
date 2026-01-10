```yaml
number: 2314
title: Communicate PEP 517 hook results via files
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/scikit-image
created_at: 2024-03-09T03:17:58Z
updated_at: 2024-03-09T11:49:58Z
url: https://github.com/astral-sh/uv/pull/2314
synced_at: 2026-01-10T14:54:43Z
```

# Communicate PEP 517 hook results via files

---

_Pull request opened by @charliermarsh on 2024-03-09 03:17_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In #1813, we were failing to install `scikit-image==0.19.3` from source in Python 3.11. Confusingly, though, the trace showed that the build command exited with status 0...

The issue is that we get results from the PEP 517 hooks by reading from `stdout` -- that is, we `print` at the end of the script, and parse the printed output on the other side.

It turns out that for `scikit-image`, in this case, there was output _after_ the wheel filename:

```
...
no previously-included directories found matching 'doc/gh-pages'
adding license file 'LICENSE.txt'
writing manifest file 'scikit_image.egg-info/SOURCES.txt'
Copying scikit_image.egg-info to build/bdist.macosx-12.6-arm64/wheel/scikit_image-0.19.3-py3.11.egg-info
running install_scripts
scikit_image-0.19.3-cp311-cp311-macosx_14_0_arm64.whl
INFO:
########### EXT COMPILER OPTIMIZATION ###########
INFO: Platform      :
  Architecture: aarch64
  Compiler    : clang

CPU baseline  :
  Requested   : 'min'
  Enabled     : NEON NEON_FP16 NEON_VFPV4 ASIMD
  Flags       : none
  Extra checks: none

CPU dispatch  :
  Requested   : 'max -xop -fma4'
  Enabled     : ASIMDHP ASIMDDP ASIMDFHM
  Generated   : none
INFO: CCompilerOpt.cache_flush[864] : write cache to path -> /private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmp5ZPIbv/built-wheels-v0/pypi/scikit-image/0.19.3/hLW_f7wWeGDOPRlSazQXw/scikit-image-0.19.3.tar.gz/build/temp.macosx-12.6-arm64-3.11/ccompiler_opt_cache_ext.py
```

We need the `scikit_image-0.19.3-cp311-cp311-macosx_14_0_arm64.whl` line, but we were failing to find it due to all the extra output at the end (presumedly, some kind of `atexit` logging).

This PR modifies the hooks to instead write their results to files that are passed in by the parent. On the other end, we then read the results back from disk. This makes it much more robust to "other" output in the script.

Closes https://github.com/astral-sh/uv/issues/1813.

## Test Plan

Ran `cargo run pip install scikit-image==0.19.3 --reinstall --no-cache-dir` on Python 3.11.


---

_Label `bug` added by @charliermarsh on 2024-03-09 03:18_

---

_@charliermarsh reviewed on 2024-03-09 03:19_

---

_Review comment by @charliermarsh on `crates/uv-build/src/lib.rs`:736 on 2024-03-09 03:19_

These don't _need_ deterministic names (we pass them into the script anyway) but I figured it would help debugging.

---

_@zanieb approved on 2024-03-09 03:22_

Makes sense!

---

_Merged by @charliermarsh on 2024-03-09 11:49_

---

_Closed by @charliermarsh on 2024-03-09 11:49_

---

_Branch deleted on 2024-03-09 11:49_

---
