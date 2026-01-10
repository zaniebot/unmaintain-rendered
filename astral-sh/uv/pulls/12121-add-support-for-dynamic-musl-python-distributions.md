```yaml
number: 12121
title: Add support for dynamic musl Python distributions on x86-64 Linux
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/musl
created_at: 2025-03-11T20:59:22Z
updated_at: 2025-03-12T08:55:39Z
url: https://github.com/astral-sh/uv/pull/12121
synced_at: 2026-01-10T11:10:39Z
```

# Add support for dynamic musl Python distributions on x86-64 Linux

---

_Pull request opened by @zanieb on 2025-03-11 20:59_

Following the upstream release and #12120, removes gating preventing installation of the managed musl Python versions.

Of note

- The filtering of musl Python distributions has moved from the Rust runtime to the metadata fetcher
- The filtering is now conditional on the PBS release date, removing all old static musl distributions
- We could support the `+static` musl downloads in the future; right now, they are deprioritized when selecting a variant
- I added test to CI which uses Alpine and installs numpy

---

_Renamed from "Remove filtering of musl Python distributions" to "Add support for dynamic musl Python distributions on x86-64 Linux" by @zanieb on 2025-03-11 21:27_

---

_@zanieb reviewed on 2025-03-11 21:27_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:62 on 2025-03-11 21:27_

We still don't provide ARM distributions

---

_Marked ready for review by @zanieb on 2025-03-11 22:12_

---

_Review requested from @charliermarsh by @zanieb on 2025-03-11 22:12_

---

_Review comment by @geofft on `.github/workflows/ci.yml`:763 on 2025-03-11 22:57_

Could this potentially affect real-world users?

---

_@geofft approved on 2025-03-11 22:57_

LGTM. download-metadata.json only has old musl distributions removed and everything else unchanged.

---

_@zanieb reviewed on 2025-03-11 23:11_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:763 on 2025-03-11 23:11_

Yes, but it'll just say "No Python download found for request" (as we do for non-musl distributions) instead of the special case "uv does not support musl downloads yet"

---

_Merged by @zanieb on 2025-03-11 23:14_

---

_Closed by @zanieb on 2025-03-11 23:14_

---

_Branch deleted on 2025-03-11 23:14_

---

_@konstin reviewed on 2025-03-12 08:55_

:tada: 

---
