```yaml
number: 4623
title: Normalize fork markers
type: pull_request
state: merged
author: konstin
labels:
  - tracing
  - preview
assignees: []
merged: true
base: main
head: konsti/normalize-fork-markers
created_at: 2024-06-28T15:31:11Z
updated_at: 2024-06-28T15:55:50Z
url: https://github.com/astral-sh/uv/pull/4623
synced_at: 2026-01-10T13:48:28Z
```

# Normalize fork markers

---

_Pull request opened by @konstin on 2024-06-28 15:31_

Looks much better than #4618:

```
DEBUG Pre-fork split universal took 0.644s
DEBUG Split python_version >= '3.12' and platform_machine == 'aarch64' and platform_system == 'Darwin' and platform_system == 'Linux' took 0.659s
DEBUG Split python_version == '3.9' and platform_machine == 'arm64' and platform_system == 'Darwin' took 0.291s
```

---

_Review requested from @BurntSushi by @konstin on 2024-06-28 15:31_

---

_Review requested from @ibraheemdev by @konstin on 2024-06-28 15:31_

---

_@konstin reviewed on 2024-06-28 15:32_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:1725 on 2024-06-28 15:32_

Undoing this change since we don't need it on every package, we only need it once per fork.

---

_Label `tracing` added by @konstin on 2024-06-28 15:32_

---

_Label `preview` added by @konstin on 2024-06-28 15:32_

---

_@ibraheemdev approved on 2024-06-28 15:41_

---

_@BurntSushi approved on 2024-06-28 15:53_

---

_Merged by @konstin on 2024-06-28 15:55_

---

_Closed by @konstin on 2024-06-28 15:55_

---

_Branch deleted on 2024-06-28 15:55_

---
