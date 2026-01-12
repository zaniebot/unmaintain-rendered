```yaml
number: 11801
title: Discover registry PEP 514 Pythons cross 32/64-bit
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-gh11217
created_at: 2025-02-26T15:37:02Z
updated_at: 2025-03-03T14:46:01Z
url: https://github.com/astral-sh/uv/pull/11801
synced_at: 2026-01-12T16:10:00Z
```

# Discover registry PEP 514 Pythons cross 32/64-bit

---

_@konstin_

Fixes #11217

By default, a 64-bit uv does not see a 32-bit global (HKLM) installation of Python in the registry (https://github.com/astral-sh/uv/issues/11217). To work around this, we manually request both 32-bit and 64-bit access using registry access flags (https://peps.python.org/pep-0514/#sample-code). The flags have no effect on 32-bit (https://stackoverflow.com/a/12796797/3549270).

This effect is that there is an asymmetry between discovery modes: For the registry-based discovery using PEP 514, we discover both 32-bit and 64-bit Pythons, while for managed installations, we are stricter and only discover those matching in bit-ness.

I tested this manually with an additional 32-bit installation of CPython on a 64-bit machine and windows with 32-bit and 64-bit (x86_64 and i686) builds of uv.

---

_Label `bug` added by @konstin on 2025-02-26 15:37_

---

_Review requested from @Gankra by @konstin on 2025-02-26 15:37_

---

_@charliermarsh approved on 2025-03-03 03:37_

---

_@Gankra approved on 2025-03-03 14:31_

Looks good to me -- random thoughts:

* It would be nice if the 64-bit pythons had priority over the 32-bit ones (if they're newer versions) but this is so narrow as to be irrelevant
* On 32-bit Windows I guess this means we'll discover the same pythons twice but they should get deduped somewhere in the pipeline so that's fine.

---

_Merged by @konstin on 2025-03-03 14:46_

---

_Closed by @konstin on 2025-03-03 14:46_

---

_Branch deleted on 2025-03-03 14:46_

---
