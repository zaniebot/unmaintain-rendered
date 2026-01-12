```yaml
number: 12012
title: "Add `--marker` flag to `uv add`"
type: pull_request
state: merged
author: thejchap
labels:
  - enhancement
assignees: []
merged: true
base: main
head: feature/req-markers
created_at: 2025-03-06T18:01:07Z
updated_at: 2025-03-11T15:29:36Z
url: https://github.com/astral-sh/uv/pull/12012
synced_at: 2026-01-12T16:10:06Z
```

# Add `--marker` flag to `uv add`

---

_@thejchap_

## Summary

Add a `--marker` flag to `uv add` which applies a marker to all given requirements.

Example:

```
$ uv-debug add --marker "platform_machine == 'x86_64'" \
    "anyio>=2.31.0" \
    "iniconfig>=2; sys_platform != 'win32'" \
    "numpy>1.19; sys_platform == 'win32'"
```

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = [
    "anyio>=2.31.0 ; platform_machine == 'x86_64'",
    "iniconfig>=2 ; platform_machine == 'x86_64' and sys_platform != 'win32'",
    "numpy>1.19 ; platform_machine == 'x86_64' and sys_platform == 'win32'",
]
```

Fixes https://github.com/astral-sh/uv/issues/11987


## Test Plan

Added snapshot tests

---

_Renamed from "add --marker flag to  command (#11987)" to "add --marker flag to `add` command (#11987)" by @thejchap on 2025-03-06 18:05_

---

_@thejchap reviewed on 2025-03-06 18:35_

---

_Review comment by @thejchap on `crates/uv/src/commands/project/add.rs`:894 on 2025-03-06 18:35_

should this be `and`-ed with an existing marker? is there a more elegant way to do this?

---

_@zanieb reviewed on 2025-03-06 19:23_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:894 on 2025-03-06 19:23_

Ah that's an interesting question. I think so?

I think the proper idiom is like `marker.map(|inner| inner.and(marker)).or(marker)`

---

_Comment by @zanieb on 2025-03-06 19:23_

If you add some test cases it'll help me tell if this looks right.

---

_Renamed from "add --marker flag to `add` command (#11987)" to "[WIP] Add --marker flag to `add` command (#11987)" by @thejchap on 2025-03-10 15:25_

---

_Comment by @thejchap on 2025-03-10 15:25_

@zanieb added a couple [tests](https://github.com/astral-sh/uv/pull/12012/files#diff-120ba84554abf504ef6d44e89e1dd7c9caa37f81842074fe70daf981bc393b71R4741)

---

_Marked ready for review by @thejchap on 2025-03-10 15:26_

---

_Review requested from @konstin by @konstin on 2025-03-10 16:16_

---

_Assigned to @konstin by @konstin on 2025-03-10 16:16_

---

_Renamed from "[WIP] Add --marker flag to `add` command (#11987)" to "Add --marker flag to `add` command (#11987)" by @thejchap on 2025-03-10 18:19_

---

_Label `enhancement` added by @konstin on 2025-03-11 14:33_

---

_Renamed from "Add --marker flag to `add` command (#11987)" to "Add `--marker` flag to `uv add`" by @konstin on 2025-03-11 14:33_

---

_@konstin approved on 2025-03-11 14:40_

Thanks!

---

_Comment by @thejchap on 2025-03-11 14:59_

@konstin thanks for the help on the tests + docs 🙏

---

_Merged by @konstin on 2025-03-11 15:29_

---

_Closed by @konstin on 2025-03-11 15:29_

---
