```yaml
number: 1956
title: "Distribution `ty==0.0.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform"
type: issue
state: closed
author: Winipedia
labels:
  - release
assignees: []
created_at: 2025-12-16T19:52:57Z
updated_at: 2025-12-16T20:36:52Z
url: https://github.com/astral-sh/ty/issues/1956
synced_at: 2026-01-10T01:55:00Z
```

# Distribution `ty==0.0.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

---

_Issue opened by @Winipedia on 2025-12-16 19:52_

### Summary

uv sync
Resolved 64 packages in 1ms
error: Distribution `ty==0.0.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_39_x86_64`), but `ty` (v0.0.1) only has wheels for the following platforms: `manylinux_2_17_armv7l`, `manylinux_2_17_ppc64`, `manylinux_2_17_i686`, `manylinux_2_17_s390x`, `manylinux2014_armv7l`, `manylinux2014_ppc64`, `manylinux2014_i686`, `manylinux2014_s390x`, `musllinux_1_2_aarch64`, `musllinux_1_2_x86_64`, `macosx_10_12_x86_64`, `macosx_11_0_arm64`, `win32`, `win_amd64`, `win_arm64`; consider adding "sys_platform == 'linux' and platform_machine == 'x86_64'" to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels

### Version

0.0.1

---

_Comment by @lacabra on 2025-12-16 19:55_

+1 

---

_Comment by @zanieb on 2025-12-16 19:56_

This partially published because some files for 0.0.1 were posted by the previous owner of ty. We're re-releasing as 0.0.2.

---

_Label `release` added by @AlexWaygood on 2025-12-16 19:56_

---

_Comment by @lacabra on 2025-12-16 19:59_

Amazing response time by the project maintainers, kudos to you! 🎉



---

_Comment by @zanieb on 2025-12-16 20:01_

We've also yanked 0.0.1 which should resolve errors unless you're requesting it specifically.

Sorry about the disruption :)

---

_Comment by @AlexWaygood on 2025-12-16 20:01_

We yanked 0.0.1, this should be fixed now!

---

_Closed by @AlexWaygood on 2025-12-16 20:01_

---

_Comment by @oconnor663 on 2025-12-16 20:36_

And now 0.0.2 is out.

---
