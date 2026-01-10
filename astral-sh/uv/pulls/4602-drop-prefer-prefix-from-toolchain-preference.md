```yaml
number: 4602
title: "Drop `prefer` prefix from `toolchain-preference` values"
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: zb/toolchain-preference-only
created_at: 2024-06-27T21:25:32Z
updated_at: 2024-07-02T02:07:32Z
url: https://github.com/astral-sh/uv/pull/4602
synced_at: 2026-01-10T13:48:28Z
```

# Drop `prefer` prefix from `toolchain-preference` values

---

_Pull request opened by @zanieb on 2024-06-27 21:25_

I think `--toolchain-preference system` is sufficiently clear and `--toolchain-preference prefer-system` is excessively verbose. This was discussed in the original pull request at https://github.com/astral-sh/uv/pull/4424 but because we had a case for preferring "installed managed" toolchains I was hesitant to change it. Now that I've dropped that in #4601, I think we can drop the prefix.

---

_Renamed from "Drop `prefer` prefix from `toolchain-prefix`" to "Drop `prefer` prefix from `toolchain-preference`" by @zanieb on 2024-06-27 21:25_

---

_Label `configuration` added by @zanieb on 2024-06-27 21:27_

---

_Label `preview` added by @zanieb on 2024-06-27 21:27_

---

_@charliermarsh approved on 2024-06-27 21:28_

---

_Renamed from "Drop `prefer` prefix from `toolchain-preference`" to "Drop `prefer` prefix from `toolchain-preference` values" by @zanieb on 2024-06-27 21:32_

---

_Merged by @zanieb on 2024-07-02 02:07_

---

_Closed by @zanieb on 2024-07-02 02:07_

---

_Branch deleted on 2024-07-02 02:07_

---
