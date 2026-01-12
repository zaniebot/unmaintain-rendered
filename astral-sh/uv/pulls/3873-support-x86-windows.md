```yaml
number: 3873
title: Support x86 windows
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: konsti/x86
created_at: 2024-05-28T07:51:02Z
updated_at: 2024-05-28T16:07:40Z
url: https://github.com/astral-sh/uv/pull/3873
synced_at: 2026-01-12T16:05:54Z
```

# Support x86 windows

---

_@konstin_

Add trampolines for x86 windows.

Closes https://github.com/astral-sh/uv/issues/3660.

---

_Label `enhancement` added by @konstin on 2024-05-28 07:51_

---

_Label `windows` added by @konstin on 2024-05-28 07:51_

---

_Comment by @codspeed-hq[bot] on 2024-05-28 07:56_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/x86)

### Merging #3873 will **not alter performance**

<sub>Comparing <code>konsti/x86</code> (e611ebb) with <code>main</code> (cedd18e)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Comment by @konstin on 2024-05-28 10:45_

I've confirmed that this works with portable 32-bit python 3.12 and an i686-pc-windows-msvc build, though on a 64-bit windows.

---

_Marked ready for review by @konstin on 2024-05-28 10:45_

---

_@charliermarsh approved on 2024-05-28 15:52_

We might need to update the `UnsupportedWindowsArch` message.

---

_Merged by @charliermarsh on 2024-05-28 16:07_

---

_Closed by @charliermarsh on 2024-05-28 16:07_

---

_Branch deleted on 2024-05-28 16:07_

---
