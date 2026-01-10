```yaml
number: 13575
title: "Hint at `tool.uv.required-environments`"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/hint-at-required-environments
created_at: 2025-05-21T12:28:44Z
updated_at: 2025-06-06T19:15:53Z
url: https://github.com/astral-sh/uv/pull/13575
synced_at: 2026-01-10T11:10:41Z
```

# Hint at `tool.uv.required-environments`

---

_Pull request opened by @konstin on 2025-05-21 12:28_

For the case where there was no matching wheel on sync, we previously added a note about which wheels are available vs. on which platform you are on. We extend this error message to link directly towards `tool.uv.required-environments`, which otherwise has a discovery problem.

On Linux (Setting `tool.uv.required-environments` doesn't help here either, but it's a clear example):

```
[project]
name = "debug"
version = "0.1.0"
requires-python = "==3.10.*"
dependencies = ["tensorflow-macos>=2.13.1"]
```

```
Resolved 41 packages in 24ms
error: Distribution `tensorflow-macos==2.16.2 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_39_x86_64`), but there are no wheels for the current platform, consider configuring `tool.uv.required-environments`.
hint: `tensorflow-macos` (v2.16.2) only has wheels for the following platform: `macosx_12_0_arm64`.
```

![image](https://github.com/user-attachments/assets/b6b49461-10d6-4e1d-bc0a-5d35d98e33d0)


---

_Label `error messages` added by @konstin on 2025-05-21 12:28_

---

_@konstin reviewed on 2025-05-21 12:29_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:5054 on 2025-05-21 12:29_

What's our style for toml configuration variables?

---

_Assigned to @zanieb by @zanieb on 2025-05-21 17:08_

---

_@zanieb reviewed on 2025-05-21 17:09_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:5054 on 2025-05-21 17:09_

I don't actually know. Maybe we should add it to the style guide.

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:5054 on 2025-05-21 17:11_

I wrote a funny regex to search... `".*"\.*(green|blue)\(\)` — green seems correct.

---

_@zanieb reviewed on 2025-05-21 17:11_

---

_@konstin reviewed on 2025-05-28 12:44_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:5054 on 2025-05-28 12:44_

Anything else we need here?

---

_Comment by @zanieb on 2025-06-06 18:24_

@konstin I posted some changes in https://github.com/astral-sh/uv/pull/13888

---

_@zanieb approved on 2025-06-06 19:11_

---

_Merged by @zanieb on 2025-06-06 19:15_

---

_Closed by @zanieb on 2025-06-06 19:15_

---

_Branch deleted on 2025-06-06 19:15_

---
