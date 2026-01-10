```yaml
number: 3855
title: "Update bundled Python URLs and add `\"arm\"` architecture variant"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/py
created_at: 2024-05-27T03:08:05Z
updated_at: 2024-05-27T03:33:09Z
url: https://github.com/astral-sh/uv/pull/3855
synced_at: 2026-01-10T14:32:20Z
```

# Update bundled Python URLs and add `"arm"` architecture variant

---

_Pull request opened by @charliermarsh on 2024-05-27 03:08_

## Summary

This also adds filtering for the ARM Pythons, since that needs some libc changes; and it closes https://github.com/astral-sh/uv/issues/3854 by way of adding an "arm" branch.

---

_Label `internal` added by @charliermarsh on 2024-05-27 03:08_

---

_Review comment by @charliermarsh on `crates/uv-interpreter/src/platform.rs`:134 on 2024-05-27 03:27_

This needs to be revisited more holistically; I'm just trying to fix the failure on ARM for now. But, anyway, Rust's `std::env::consts::ARCH` just returns `"arm"`, whereas we need to differentiate between ARM v6 and ARM v7 I guess? I'm also not sure whether the enum variants are intended to match some Rust or Python convention somewhere.This needs to be revisited more holistically; I'm just trying to fix the failure on ARM for now. But, anyway, Rust's `std::env::consts::ARCH` just returns `"arm"`, whereas we need to differentiate between ARM v6 and ARM v7 I guess? I'm also not sure whether the enum variants are intended to match some Rust or Python convention somewhere.

---

_@charliermarsh reviewed on 2024-05-27 03:27_

---

_Label `internal` removed by @charliermarsh on 2024-05-27 03:27_

---

_Label `bug` added by @charliermarsh on 2024-05-27 03:27_

---

_Renamed from "Update bundled Python URLs" to "Update bundled Python URLs and add `"arm"` architecture variant" by @charliermarsh on 2024-05-27 03:27_

---

_Merged by @charliermarsh on 2024-05-27 03:33_

---

_Closed by @charliermarsh on 2024-05-27 03:33_

---

_Branch deleted on 2024-05-27 03:33_

---
