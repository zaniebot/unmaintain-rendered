```yaml
number: 7263
title: Add support for managed Python 3.13 and update CPython versions
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/python-313
created_at: 2024-09-10T17:40:43Z
updated_at: 2024-10-22T22:50:25Z
url: https://github.com/astral-sh/uv/pull/7263
synced_at: 2026-01-12T16:07:45Z
```

# Add support for managed Python 3.13 and update CPython versions

---

_@zanieb_

Adds support for CPython 3.13.0rc2

Also bumps to the latest patch version of all the other CPython minor versions we support.


---

_Label `enhancement` added by @zanieb on 2024-09-10 17:40_

---

_Marked ready for review by @zanieb on 2024-09-10 18:53_

---

_@zanieb reviewed on 2024-09-10 18:54_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:335 on 2024-09-10 18:54_

Annoyed that we parse here then unparse and parse later but it seemed painful otherwise.

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:274 on 2024-09-10 18:58_

Reverted #7131 because we dropped that validation in #7264

---

_@zanieb reviewed on 2024-09-10 18:58_

---

_Renamed from "Add support for managed Python 3.13" to "Add support for managed Python 3.13 and update CPython versions" by @zanieb on 2024-09-10 18:58_

---

_@zanieb reviewed on 2024-09-10 18:59_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:170 on 2024-09-10 18:59_

Needed to add support for pre-releases throughout

---

_@zanieb reviewed on 2024-09-10 18:59_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:468 on 2024-09-10 18:59_

Just a nit here that we were printing an empty `()` when there was no flavor (e.g. for PyPy)

---

_@zanieb reviewed on 2024-09-10 19:00_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:335 on 2024-09-10 19:00_

The alternative is to hand-roll the parser like I do in the download metadata Python script.

---

_@charliermarsh approved on 2024-09-10 19:00_

Generally looks reasonable. Why `Cow` and not `&'static str`? Do we pass in `String` somewhere to create those ourselves?

---

_Comment by @zanieb on 2024-09-10 19:34_

Supersedes #7256 

---

_Merged by @zanieb on 2024-09-10 19:36_

---

_Closed by @zanieb on 2024-09-10 19:36_

---

_Branch deleted on 2024-09-10 19:36_

---

_Comment by @Mentorlanaj on 2024-10-22 22:50_

tung

---
