```yaml
number: 8663
title: Add support for installing versioned Python executables on Windows
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/python-exec-windows
created_at: 2024-10-29T15:35:50Z
updated_at: 2024-10-31T15:58:36Z
url: https://github.com/astral-sh/uv/pull/8663
synced_at: 2026-01-12T16:08:25Z
```

# Add support for installing versioned Python executables on Windows

---

_@zanieb_

Incorporating #8637 into #8458 

- Adds `python-managed` feature selection to Windows CI for `python install` tests
- Adds trampoline sniffing utilities to `uv-trampoline-builder`
- Uses a trampoline to install Python executables into the `PATH` on Windows

---

_Converted to draft by @zanieb on 2024-10-29 15:36_

---

_Label `preview` added by @zanieb on 2024-10-31 01:08_

---

_@zanieb reviewed on 2024-10-31 03:37_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:671 on 2024-10-31 03:37_

The warning is platform-dependent so let's just put this on the `PATH`

---

_Marked ready for review by @zanieb on 2024-10-31 04:37_

---

_Review requested from @konstin by @zanieb on 2024-10-31 04:37_

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:493 on 2024-10-31 09:39_

Does this still work when we copy, wouldn't the python interpreter be unable to find its home, esp with pbs?

---

_Review comment by @konstin on `crates/uv-python/src/managed.rs`:508 on 2024-10-31 09:42_

With the try operator (`?`), we shouldn't ever take the `err.kind() == io::ErrorKind::NotFound` result matching branch

---

_Review comment by @konstin on `crates/uv-trampoline-builder/src/lib.rs`:92 on 2024-10-31 09:44_

nit: move the description into the `thiserror` `error` attribute

---

_@konstin approved on 2024-10-31 09:54_

The `python3.13.exe` in PATH works, but IDEs such as pycharm or vs code don't detect it.

---

_@zanieb reviewed on 2024-10-31 13:18_

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:493 on 2024-10-31 13:18_

This actually will only copy on Windows, I think. I'll update the utility accordingly or not use it here for clarity.

---

_Review comment by @zanieb on `crates/uv-python/src/managed.rs`:508 on 2024-10-31 13:18_

Thanks!

---

_@zanieb reviewed on 2024-10-31 13:18_

---

_@zanieb reviewed on 2024-10-31 13:19_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:92 on 2024-10-31 13:19_

There are a bunch of these and I wasn't sure it was worth the effort of extra `thiserror` variants. Wdyt?

---

_@konstin reviewed on 2024-10-31 13:35_

---

_Review comment by @konstin on `crates/uv-trampoline-builder/src/lib.rs`:92 on 2024-10-31 13:35_

I'm always using thiserror: The biggest benefit for me is that moves the error message strings out of the main logic and allows reusing the error in another place.

If needed, it also allows matching on the error type in upstream. I tend to see the error enum variants as almost free of cost. It's less pronounced here since we're already using an error enum (and obviously it makes sense to often pass string snippets into error variants as we do a lot), this is mostly out of experience in maturin where it's all anyhow and `.context()` and i wished i had used thiserror consistently.

---

_@zanieb reviewed on 2024-10-31 13:43_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:92 on 2024-10-31 13:43_

Yeah the nuanced part here is that I don't think you'd ever want to match on different error cases here, i.e., it's more work for a consumer to match on the errors and they'll have to learn that they need to combine a bunch of these cases when they're semantically equivalent to a caller, but I'll try to move some more into the error variant.

---

_@zanieb reviewed on 2024-10-31 13:55_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:92 on 2024-10-31 13:55_

Actually really satisfying to rewrite these.. lol

---

_Renamed from "Use trampolines for Python executables on Windows" to "Add support for installing versioned Python executables on Windows" by @zanieb on 2024-10-31 14:22_

---

_Merged by @zanieb on 2024-10-31 15:58_

---

_Closed by @zanieb on 2024-10-31 15:58_

---

_Branch deleted on 2024-10-31 15:58_

---
