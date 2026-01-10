```yaml
number: 4618
title: Log fork timings
type: pull_request
state: merged
author: konstin
labels:
  - tracing
  - preview
assignees: []
merged: true
base: main
head: konsti/log-fork-timing
created_at: 2024-06-28T14:37:41Z
updated_at: 2024-06-28T15:31:24Z
url: https://github.com/astral-sh/uv/pull/4618
synced_at: 2026-01-10T13:48:28Z
```

# Log fork timings

---

_Pull request opened by @konstin on 2024-06-28 14:37_

This includes a functional change, we now skip the forked state pop/push if we didn't fork.

From transformers:

```
DEBUG Pre-fork split universal took 0.036s
DEBUG Split python_version >= '3.10' and python_version >= '3.10' and platform_system == 'Darwin' and python_version >= '3.11' and python_version >= '3.12' and python_version >= '3.6' and platform_system == 'Linux' and platform_machine == 'aarch64' took 0.048s
DEBUG Split python_version <= '3.9' and platform_system == 'Darwin' and platform_machine == 'arm64' and python_version >= '3.7' and python_version >= '3.8' and python_version >= '3.9' took 0.038s
```

The messages could use simplification from https://github.com/astral-sh/uv/issues/4536

We can consider nested spans in the future but this works nicely for now.


---

_Label `tracing` added by @konstin on 2024-06-28 14:37_

---

_Label `preview` added by @konstin on 2024-06-28 14:37_

---

_Review requested from @BurntSushi by @konstin on 2024-06-28 14:37_

---

_@BurntSushi approved on 2024-06-28 14:39_

Yeah I buy this!

---

_Merged by @konstin on 2024-06-28 14:45_

---

_Closed by @konstin on 2024-06-28 14:45_

---

_Branch deleted on 2024-06-28 14:45_

---

_Comment by @ibraheemdev on 2024-06-28 15:24_

Can we call `marker::normalize` before logging? We should already be able to simplify that debug output quite a bit.

---

_Comment by @konstin on 2024-06-28 15:31_

Thanks, that's much better!

---
