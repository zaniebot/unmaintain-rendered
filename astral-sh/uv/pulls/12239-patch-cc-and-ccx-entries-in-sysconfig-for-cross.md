```yaml
number: 12239
title: "Patch `CC` and `CCX` entries in sysconfig for cross-compiled `aarch64` Python distributions"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/cpp
created_at: 2025-03-17T14:00:39Z
updated_at: 2025-05-04T12:26:53Z
url: https://github.com/astral-sh/uv/pull/12239
synced_at: 2026-01-10T11:10:39Z
```

# Patch `CC` and `CCX` entries in sysconfig for cross-compiled `aarch64` Python distributions

---

_Pull request opened by @zanieb on 2025-03-17 14:00_

Closes https://github.com/astral-sh/uv/issues/12207


---

_Label `bug` added by @zanieb on 2025-03-17 14:00_

---

_@charliermarsh approved on 2025-03-17 14:02_

---

_Renamed from "Replace `CXX` entries with `gnu-g++` in sysconfig" to "Patch `CC` and `CCX` entries in sysconfig for cross-compiled Python distributions" by @zanieb on 2025-03-17 14:04_

---

_Comment by @zanieb on 2025-03-17 14:06_

It turns out this is relevant for CC too

```
❯ docker run -it --rm ghcr.io/astral-sh/uv:0.6.5-bookworm-slim /bin/bash -c "uv run -p 3.13 -m sysconfig | grep CC"
	CC = "/usr/bin/aarch64-linux-gnu-gcc"
```

We just were never patching these (and it didn't matter because people are happy with GCC)

I broke this upstream in https://github.com/astral-sh/python-build-standalone/pull/545 because I defined `CXX` as the host compiler instead of the target per this comment https://github.com/astral-sh/python-build-standalone/pull/545#issuecomment-2689183313 — we can change that upstream.

I'm unsure if it'd be disruptive to ship this change. It seems more correct.

---

_Renamed from "Patch `CC` and `CCX` entries in sysconfig for cross-compiled Python distributions" to "Patch `CC` and `CCX` entries in sysconfig for cross-compiled `aarch64` Python distributions" by @zanieb on 2025-03-17 14:08_

---

_Comment by @zanieb on 2025-03-17 14:08_

This is also going to be a problem for other cross-compiles like, s390x, e.g., the values will be

```
  host_cc: /usr/bin/x86_64-linux-gnu-gcc
  host_cxx: /usr/bin/x86_64-linux-gnu-g++
  target_cc: /usr/bin/s390x-linux-gnu-gcc
```

---

_Comment by @zanieb on 2025-03-17 14:16_

Here's the upstream fix https://github.com/astral-sh/python-build-standalone/pull/563

Though we may still want to pursue this — we'll need to handle more patterns for it to be robust.

---

_Converted to draft by @zanieb on 2025-03-17 14:17_

---

_Comment by @samypr100 on 2025-03-22 01:42_

If we could define the authoritative list of variants being [targets.yml](https://github.com/astral-sh/python-build-standalone/blob/main/cpython-unix/targets.yml), I wonder if we could use it to autogen the replacements on uv side automatically

---

_Marked ready for review by @zanieb on 2025-04-16 13:34_

---

_Merged by @zanieb on 2025-04-16 13:34_

---

_Closed by @zanieb on 2025-04-16 13:34_

---

_Branch deleted on 2025-04-16 13:34_

---

_@zanieb reviewed on 2025-04-30 16:56_

---

_Review comment by @zanieb on `crates/uv-python/src/sysconfig/mod.rs`:460 on 2025-04-30 16:56_

Ugh, it's wrong in the snapshot even.

---

_@zanieb reviewed on 2025-04-30 16:57_

---

_Review comment by @zanieb on `crates/uv-python/src/sysconfig/mod.rs`:460 on 2025-04-30 16:57_

Ugh, it's a `BTreeMap`

---

_Review comment by @samypr100 on `crates/uv-python/src/sysconfig/mod.rs`:460 on 2025-04-30 17:09_

😅 Sorry, I didn't think about the multi-arch scenario originally

---

_@samypr100 reviewed on 2025-04-30 17:09_

---
