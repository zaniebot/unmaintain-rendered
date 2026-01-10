```yaml
number: 15937
title: "Move Python install mirror parsing to `EnvironmentOptions`"
type: pull_request
state: merged
author: andrey-berenda
labels:
  - internal
assignees: []
merged: true
base: main
head: move-install_mirrors-to-EnvironmentOptions
created_at: 2025-09-18T20:37:48Z
updated_at: 2025-09-25T15:35:42Z
url: https://github.com/astral-sh/uv/pull/15937
synced_at: 2026-01-10T06:36:15Z
```

# Move Python install mirror parsing to `EnvironmentOptions`

---

_Pull request opened by @andrey-berenda on 2025-09-18 20:37_

## Summary

Add install_mirrors to EnvironmentOptions
Relates #14720

## Test Plan

Tests with existing tests


---

_Marked ready for review by @andrey-berenda on 2025-09-18 20:53_

---

_Review requested from @zanieb by @konstin on 2025-09-19 07:32_

---

_@zanieb reviewed on 2025-09-20 15:12_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:1011 on 2025-09-20 15:12_

Our canonical name for this is `combine`, e.g., search for `pub fn combine(self, other: Self) -> Self {`

---

_@zanieb reviewed on 2025-09-20 15:17_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:1029 on 2025-09-20 15:17_

I'm pretty confused by this code (both before and after the changes). I think we might need to refactor it.

I think part of the problem is that `args` does not have a `PythonInstallMirrors` type? One solution is to implement something like `PythonInstallMirrors::from_args` so you can use your combine method instead of reimplementing it here.

---

_Comment by @zanieb on 2025-09-20 15:20_

This looks pretty good, thank you! I think I just want to see some tweaks where `args` need to be merged.

---

_@andrey-berenda reviewed on 2025-09-20 21:15_

---

_Review comment by @andrey-berenda on `crates/uv/src/settings.rs`:1029 on 2025-09-20 21:15_

I added install_mirrors method to PythonInstallArgs and PythonUpgradeArgs
is it ok?

If I will add PythonInstallMirrors::from_args then uv-settings will depend on uv-cli which I think is not so good

---

_@andrey-berenda reviewed on 2025-09-20 21:16_

---

_Review comment by @andrey-berenda on `crates/uv-settings/src/settings.rs`:1011 on 2025-09-20 21:16_

Renamed

---

_Review requested from @zanieb by @andrey-berenda on 2025-09-20 21:49_

---

_@zanieb reviewed on 2025-09-22 21:59_

---

_Review comment by @zanieb on `crates/uv-settings/src/lib.rs`:588 on 2025-09-22 21:59_

We should fail if these are invalid unicode and ignore them if they are set to empty values.

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:1029 on 2025-09-22 21:59_

This seems reasonable.

---

_@zanieb reviewed on 2025-09-22 21:59_

---

_@zanieb approved on 2025-09-22 23:52_

lgtm once CI is passing

---

_Merged by @zanieb on 2025-09-25 15:35_

---

_Closed by @zanieb on 2025-09-25 15:35_

---

_Label `internal` added by @zanieb on 2025-09-25 15:35_

---

_Renamed from "Add install_mirrors to EnvironmentOptions" to "Move Python install mirror parsing to `EnvironmentOptions`" by @zanieb on 2025-09-25 15:35_

---
