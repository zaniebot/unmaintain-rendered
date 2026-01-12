```yaml
number: 9723
title: Preserve directory-level standalone build symlinks
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/sym
created_at: 2024-12-08T16:09:49Z
updated_at: 2024-12-10T20:41:30Z
url: https://github.com/astral-sh/uv/pull/9723
synced_at: 2026-01-12T16:08:57Z
```

# Preserve directory-level standalone build symlinks

---

_@charliermarsh_

## Summary

This PR improves our "don't fully resolve symlinks" behavior for `python-build-standalone` builds based on learnings from https://github.com/indygreg/python-build-standalone/issues/380#issuecomment-2526575235.

Specifically, we can now robustly detect whether a target executable will lead to a valid `prefix` or not, and iteratively resolve symlinks until we find a valid target executable.

## Test Plan

### Direct symlink to `python`

Correctly resolves to the symlink target, rather than the symlink itself.

```
❯ ln -s /Users/crmarsh/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python foo
❯ cargo run venv --python ./foo
❯ cat .venv/pyvenv.cfg
home = /Users/crmarsh/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin
implementation = CPython
uv = 0.5.7
version_info = 3.12.6
include-system-site-packages = false
prompt = uv
❯ .venv/bin/python -c "import sys"
```

### Symlink to the Python installation

Correctly does _not_ resolve the symlink.

```
❯ ln -s /Users/crmarsh/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none bar
❯ cargo run venv --python ./bar
❯ cat .venv/pyvenv.cfg
home = /Users/crmarsh/workspace/uv/bar/bin
implementation = CPython
uv = 0.5.7
version_info = 3.12.6
include-system-site-packages = false
prompt = uv
❯ .venv/bin/python -c "import sys"
```

### Direct symlink to `python` in a symlinked Python installation

Correctly resolves the direct symlink, but not the symlink of the Python installation.

```
❯ ln -s bar/bin/python baz
❯ cargo run venv --python ./baz
❯ cat .venv/pyvenv.cfg
home = /Users/crmarsh/workspace/uv/bar/bin
implementation = CPython
uv = 0.5.7
version_info = 3.12.6
include-system-site-packages = false
prompt = uv
❯ .venv/bin/python -c "import sys"
```


---

_Label `enhancement` added by @charliermarsh on 2024-12-08 16:09_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-08 16:10_

---

_Review requested from @konstin by @charliermarsh on 2024-12-08 16:10_

---

_Comment by @charliermarsh on 2024-12-08 16:10_

This is pretty handy because... symlinking the full directory is much more common, and useful?

---

_Converted to draft by @charliermarsh on 2024-12-08 16:13_

---

_Marked ready for review by @charliermarsh on 2024-12-08 17:36_

---

_Comment by @charliermarsh on 2024-12-09 01:28_

Based on what I've learned [here](https://github.com/indygreg/python-build-standalone/issues/380#issuecomment-2526575235), I think the right thing to do would be: check if we can find `STDLIB_LANDMARKS` in any parent directory of the executable. If not, then we should iteratively resolve it until we find the first resolved symlink that _does_ have `STDLIB_LANDMARKS`. This would make it a non-issue!

---

_@konstin approved on 2024-12-09 09:05_

---

_Comment by @charliermarsh on 2024-12-09 15:00_

(I think I'll try rewriting to use the logic I mentioned above, since it will also work in more nuanced cases, like a direct symlink to a Python executable that's a symlink to a Python installation.)

---

_Review requested from @konstin by @charliermarsh on 2024-12-09 18:12_

---

_@konstin approved on 2024-12-10 08:46_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:65 on 2024-12-10 14:42_

Is it still correct to only gate this to standalone interpreters?

---

_@zanieb reviewed on 2024-12-10 14:42_

---

_@zanieb approved on 2024-12-10 14:42_

---

_@charliermarsh reviewed on 2024-12-10 20:04_

---

_Review comment by @charliermarsh on `crates/uv-virtualenv/src/virtualenv.rs`:65 on 2024-12-10 20:04_

It's possible that this would be equally applicable to other interpreters, but I'm a little more hesitant... The `else` behavior matches the standard library, and we're just less familiar with how those "other interpreters" behave.

---

_@zanieb reviewed on 2024-12-10 20:15_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:65 on 2024-12-10 20:15_

Okay. Fine with me, this logic seems very "correct" though.

---

_Merged by @charliermarsh on 2024-12-10 20:41_

---

_Closed by @charliermarsh on 2024-12-10 20:41_

---

_Branch deleted on 2024-12-10 20:41_

---
