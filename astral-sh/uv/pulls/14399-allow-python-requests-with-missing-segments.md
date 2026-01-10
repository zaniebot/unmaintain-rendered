```yaml
number: 14399
title: Allow Python requests with missing segments
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/parse-key
created_at: 2025-07-01T18:29:09Z
updated_at: 2025-08-13T16:03:11Z
url: https://github.com/astral-sh/uv/pull/14399
synced_at: 2026-01-10T06:44:33Z
```

# Allow Python requests with missing segments

---

_Pull request opened by @zanieb on 2025-07-01 18:29_

This allows `PythonDownloadRequest` which is used for parsing general install key requests to have missing segments, which unblocks requests like `windows-aarch64` or `cpython-linux` (whereas before those would require `any-any-windows-aarch64` and `cpython-any-linux` respectively). We still require strict ordering of segments.

Previously, we only allowed missing segments at the end of the key.

This uses a state machine for parsing, which is quite a bit more complicated.

I'm a little hesitant about the possibility that this regresses error messages and the complexity of the implementation, but `uv run -p aarch64` seems valuable following #13724. The alternative to this would probably be to make these explicit in various places? e.g., expose `--python-arch`, `--python-libc`, and `--python-os`? Or make `--python-platform` (which already exists) accept a subset of the keys?

There is a possibility of regressions here, e.g., if something matches this parser it will not fallback to the `PythonRequest::ExecutableName` case and we've made this parser more permissive, but I think that should be quite rare?

---

_Renamed from "zb/parse key" to "Allow Python requests with missing segments" by @zanieb on 2025-07-01 18:29_

---

_@zanieb reviewed on 2025-07-02 16:29_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1626 on 2025-07-02 16:29_

I used these here as a PoC / integration test!

---

_Marked ready for review by @zanieb on 2025-07-02 16:51_

---

_@BurntSushi reviewed on 2025-07-02 16:59_

---

_Review comment by @BurntSushi on `crates/uv-python/src/downloads.rs`:715 on 2025-07-02 16:59_

This LGTM. Once I figured out how to read one state transition (i.e., the differing behavior based on whether the value parsed or not), I found it easy to pattern match that understanding to the other transitions. So there's a fair bit of code, but it follows a nice repeatable structure that I find easy to digest.

So, nice job!

---

_@zanieb reviewed on 2025-07-02 17:01_

---

_Review comment by @zanieb on `crates/uv-python/src/downloads.rs`:715 on 2025-07-02 17:01_

<3 I could document how the machine works a little

---

_Merged by @zanieb on 2025-08-13 16:03_

---

_Closed by @zanieb on 2025-08-13 16:03_

---

_Branch deleted on 2025-08-13 16:03_

---
