```yaml
number: 5033
title: "pip compile: exclude --upgrade-package when option and value are passed as a single argument"
type: pull_request
state: merged
author: skshetry
labels:
  - compatibility
assignees: []
merged: true
base: main
head: exclude-upgrade-package-header
created_at: 2024-07-13T05:04:36Z
updated_at: 2024-07-13T17:19:22Z
url: https://github.com/astral-sh/uv/pull/5033
synced_at: 2026-01-12T16:06:35Z
```

# pip compile: exclude --upgrade-package when option and value are passed as a single argument

---

_@skshetry_

This excludes `--upgrade-package` from `compile_command` when value and option are passed as a single argument. Eg:

```console
--upgrade-package=package
-P=package
-Ppackage
```

I missed this on #5032.
Fixes #5031.

## Test Plan

Tested locally

---

_@skshetry reviewed on 2024-07-13 05:06_

---

_Review comment by @skshetry on `crates/uv/src/commands/pip/compile.rs`:551 on 2024-07-13 05:06_

This differs from `--find-links` above for short option, to support `-Ppackage` form of argument.

https://github.com/astral-sh/uv/blob/93563ba1979e35903b235e8bf79cc5bf564f3c13/crates/uv/src/commands/pip/compile.rs#L531

It is currently checking for `-f=`, but this won't work for arguments passed as `-f<uri>` option.

I can roll back this change if you don't want to support that style of argument, or we can modify the `--find-links` option to also exclude `-f<uri>` (can also fix in a separate PR).





---

_@skshetry reviewed on 2024-07-13 05:09_

---

_Review comment by @skshetry on `crates/uv/src/commands/pip/compile.rs`:551 on 2024-07-13 05:09_

But this raw argument check is not enough to exclude arguments. Eg:
```console
uv pip compile ... -vPruff
```

or, 
```console
uv pip compile ... -vv
```

---

_Renamed from "pip compile: exclude --upgrade-package when options and values are pa…" to "pip compile: exclude --upgrade-package when option and value are passed as a single argument" by @skshetry on 2024-07-13 05:10_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-13 15:04_

---

_@charliermarsh reviewed on 2024-07-13 16:35_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/compile.rs`:551 on 2024-07-13 16:35_

Yeah let's catch `-P=` and `-P<package>` and same for find-links.

---

_@charliermarsh approved on 2024-07-13 16:46_

---

_Merged by @charliermarsh on 2024-07-13 16:51_

---

_Closed by @charliermarsh on 2024-07-13 16:51_

---

_Branch deleted on 2024-07-13 16:53_

---

_Comment by @charliermarsh on 2024-07-13 17:19_

Thank you!

---

_Label `compatibility` added by @charliermarsh on 2024-07-13 17:19_

---
