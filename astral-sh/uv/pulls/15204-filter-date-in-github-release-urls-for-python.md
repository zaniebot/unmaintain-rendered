```yaml
number: 15204
title: " Filter date in GitHub release URLs for `python_install_no_cache` test"
type: pull_request
state: merged
author: ubaidsk
labels:
  - testing
assignees: []
merged: true
base: main
head: ubaid/pr/filter_date_for_python_install_no_cache
created_at: 2025-08-10T17:02:35Z
updated_at: 2025-08-12T05:31:41Z
url: https://github.com/astral-sh/uv/pull/15204
synced_at: 2026-01-12T16:11:38Z
```

#  Filter date in GitHub release URLs for `python_install_no_cache` test

---

_@ubaidsk_

## Summary

fixes https://github.com/astral-sh/uv/issues/15172

This change adds a regex filter to normalize dates in GitHub release URLs within the `python_install_no_cache` test snapshot.

**Problem:**
The test was hardcoding the date `20250808` in the expected error message URL:

```console
https://github.com/astral-sh/python-build-standalone/releases/download/20250808/cpython-3.12.[PATCH]-[DATE]-[PLATFORM].tar.gz
```

This creates a maintenance burden as the snapshot would need to be updated whenever the underlying Python release date changes.

**Solution:**
Added a regex filter `r"releases/download/\d{8}/"` → `"releases/download/[DATE]/"` to replace any 8-digit date in the GitHub release URL path with a generic `[DATE]` placeholder.

**Result:**
The test is now resilient to new Python releases and won't require snapshot updates when the underlying release date changes. The error message now consistently shows:

```console
https://github.com/astral-sh/python-build-standalone/releases/download/[DATE]/cpython-3.12.[PATCH]-[DATE]-[PLATFORM].tar.gz
```

## Test Plan

`python_install` tests seem to pass ✅ 

```console
$ cargo test --package uv --test it -- python_install
   Compiling uv-cli v0.0.1 (/home/ubaid/projects/uv/crates/uv-cli)
   Compiling uv v0.8.8 (/home/ubaid/projects/uv/crates/uv)
    Finished `test` profile [unoptimized + debuginfo] target(s) in 19.04s
     Running tests/it/main.rs (target/debug/deps/it-14d47eb0324a8a0a)

running 30 tests
test python_install::python_install_unknown ... ok
test network::python_install_io_error ... ok
test network::python_install_http_500 ... ok
test python_install::python_install_invalid_request ... ok
test python_install::python_install_broken_link ... ok
test python_install::python_install_preview_no_bin ... ok
test python_install::regression_cpython ... ok
test python_install::uninstall_last_patch ... ok
test python_install::install_no_transparent_upgrade_with_venv_patch_specification ... ok
test python_install::install_lower_patch_automatically ... ok
test python_install::uninstall_highest_patch ... ok
test python_install::install_transparent_patch_upgrade_venv_module ... ok
test python_install::python_install_default_from_env ... ok
test python_install::python_install ... ok
test python_install::python_reinstall_patch ... ok
test python_install::python_install_force ... ok
test python_install::install_transparent_patch_upgrade_uv_venv ... ok
test python_install::install_multiple_patches ... ok
test python_install::python_install_314 ... ok
test python_install::python_install_default ... ok
test python_install::python_install_automatic ... ok
test python_install::python_install_freethreaded ... ok
test python_install::python_install_preview_upgrade ... ok
test python_install::python_install_no_cache ... ok
test python_install::python_install_default_preview ... ok
test python_install::python_install_preview ... ok
test python_install::python_install_minor ... ok
test python_install::python_reinstall ... ok
test python_install::python_install_cached ... ok
test python_install::python_install_multiple_patch ... ok

test result: ok. 30 passed; 0 failed; 0 ignored; 0 measured; 2207 filtered out; finished in 23.34s
```


---

_@zanieb approved on 2025-08-11 21:15_

Thanks!

---

_Label `testing` added by @zanieb on 2025-08-11 21:15_

---

_Merged by @zanieb on 2025-08-11 21:15_

---

_Closed by @zanieb on 2025-08-11 21:15_

---

_Branch deleted on 2025-08-12 05:31_

---
