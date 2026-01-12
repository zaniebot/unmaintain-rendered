```yaml
number: 14518
title: Add support for installing pyodide Pythons
type: pull_request
state: merged
author: hoodmane
labels: []
assignees: []
merged: true
base: main
head: pyodide-installation
created_at: 2025-07-09T13:56:12Z
updated_at: 2025-09-10T16:13:12Z
url: https://github.com/astral-sh/uv/pull/14518
synced_at: 2026-01-12T16:11:15Z
```

# Add support for installing pyodide Pythons

---

_@hoodmane_

- [x] Add tests


---

_@hoodmane reviewed on 2025-08-01 11:20_

---

_Review comment by @hoodmane on `crates/uv-python/src/lib.rs`:2733 on 2025-08-01 11:20_

@zanieb I tried to figure out how to sort the Emscripten interpreters after all the others but couldn't figure out how. Maybe `find_python_installation` should just discard them? I think it makes the most sense to only use them when specifically requested.

---

_@hoodmane reviewed on 2025-08-01 12:35_

---

_Review comment by @hoodmane on `crates/uv-python/src/lib.rs`:2733 on 2025-08-01 12:35_

Okay I did something slightly ad hoc...

---

_Marked ready for review by @hoodmane on 2025-08-01 12:43_

---

_Comment by @hoodmane on 2025-08-01 12:44_

@zanieb this is ready for review now.

---

_Assigned to @zanieb by @zanieb on 2025-08-01 12:48_

---

_Comment by @hoodmane on 2025-08-01 12:50_

At least assuming the CI passes...

---

_@zanieb approved on 2025-08-13 15:41_

---

_Comment by @hoodmane on 2025-08-13 16:03_

Thanks @zanieb!

---

_Merged by @zanieb on 2025-08-13 16:03_

---

_Closed by @zanieb on 2025-08-13 16:03_

---

_Branch deleted on 2025-08-13 16:13_

---

_Comment by @henryiii on 2025-09-10 14:15_

I notice iOS and Android are now supported, but I'm guessing Pyodide isn't yet (for installing)?

---

_Comment by @konstin on 2025-09-10 14:39_

Can you do `uv run -p cpython-3.13.2-emscripten-wasm32-musl python`?

---

_Comment by @zanieb on 2025-09-10 15:03_

Can you clarify what you mean?

---

_Comment by @charliermarsh on 2025-09-10 15:04_

Yes, Pyodide tags are supported.

---

_Comment by @henryiii on 2025-09-10 16:08_

Great, I'll see if I can support it then in cibuildwheel!

---

_Comment by @hoodmane on 2025-09-10 16:13_

Currently there is one more thing that needs to be addressed before uv can completely replace pyodide-build for testing against pyodide or running code with it (but not for actually building packages): we need to update uv to know about the jsdelivr index. Hopefully I'll get that done soon...

---
