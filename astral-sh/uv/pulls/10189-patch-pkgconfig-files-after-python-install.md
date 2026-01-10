```yaml
number: 10189
title: Patch pkgconfig files after Python install
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/pkg
created_at: 2024-12-27T00:35:15Z
updated_at: 2024-12-27T00:50:42Z
url: https://github.com/astral-sh/uv/pull/10189
synced_at: 2026-01-10T11:44:38Z
```

# Patch pkgconfig files after Python install

---

_Pull request opened by @charliermarsh on 2024-12-27 00:35_

## Summary

Closes https://github.com/astral-sh/uv/issues/10185.

## Test Plan

Ran `cargo run python install 3.10.15 --reinstall`; verified that `python3.pc` contained:

```
# See: man pkg-config
prefix=/Users/crmarsh/.local/share/uv/python/cpython-3.10.15-macos-aarch64-none
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
includedir=${prefix}/include

Name: Python
Description: Build a C extension for Python
Requires:
Version: 3.10
Libs.private: -ldl   -framework CoreFoundation
Libs:
Cflags: -I${includedir}/python3.10
```


---

_Label `bug` added by @charliermarsh on 2024-12-27 00:35_

---

_Label `compatibility` added by @charliermarsh on 2024-12-27 00:35_

---

_Merged by @charliermarsh on 2024-12-27 00:50_

---

_Closed by @charliermarsh on 2024-12-27 00:50_

---

_Branch deleted on 2024-12-27 00:50_

---
