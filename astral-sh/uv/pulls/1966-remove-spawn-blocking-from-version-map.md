```yaml
number: 1966
title: "Remove `spawn_blocking` from version map"
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/remove-spawn-blocking-from-version-map
created_at: 2024-02-25T14:30:15Z
updated_at: 2024-02-26T09:44:25Z
url: https://github.com/astral-sh/uv/pull/1966
synced_at: 2026-01-10T14:54:43Z
```

# Remove `spawn_blocking` from version map

---

_Pull request opened by @konstin on 2024-02-25 14:30_

I previously add `spawn_blocking` to the version map construction as it had become a bottleneck (https://github.com/astral-sh/uv/pull/1163/files#diff-704ceeaedada99f90369eac535713ec82e19550bff166cd44745d7277ecae527R116). With the zero copy deserialization, this has become so fast we don't need to move it to the thread pool anymore. I've also checked `DataWithCachePolicy` but it seems to still take a significant amount of time. Span visualization:

Resolving jupyter warm:

![image](https://github.com/astral-sh/uv/assets/6826232/692b03da-61c5-4f96-b413-199c14aa47c4)

Resolving jupyter cold:

![image](https://github.com/astral-sh/uv/assets/6826232/a6893155-d327-40c9-a83a-7c537b7c99c4)
![image](https://github.com/astral-sh/uv/assets/6826232/213556a3-a331-42db-aaf5-bdef5e0205dd)

I've also updated the instrumentation a little.

We don't seem cpu bound for the cold cache (top) and refresh case (bottom) from jupyter:

![image](https://github.com/astral-sh/uv/assets/6826232/cb976add-3d30-465a-a470-8490b7b6caea)

![image](https://github.com/astral-sh/uv/assets/6826232/d7ecb745-dd2d-4f91-939c-2e46b7c812dd)



---

_Label `performance` added by @konstin on 2024-02-25 14:30_

---

_Review requested from @BurntSushi by @konstin on 2024-02-25 14:30_

---

_@zanieb approved on 2024-02-25 16:03_

---

_@BurntSushi approved on 2024-02-25 20:13_

Lovely!

---

_Comment by @charliermarsh on 2024-02-25 21:10_

The dreaded `thread 'main' has overflowed its stack` on Windows debug builds.

---

_Merged by @charliermarsh on 2024-02-26 09:44_

---

_Closed by @charliermarsh on 2024-02-26 09:44_

---

_Branch deleted on 2024-02-26 09:44_

---
