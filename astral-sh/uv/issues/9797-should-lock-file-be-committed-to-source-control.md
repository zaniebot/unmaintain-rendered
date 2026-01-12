```yaml
number: 9797
title: Should lock file be committed to source control?
type: issue
state: closed
author: ngaloppo
labels:
  - question
assignees: []
created_at: 2024-12-11T01:29:48Z
updated_at: 2025-09-10T08:30:29Z
url: https://github.com/astral-sh/uv/issues/9797
synced_at: 2026-01-12T15:59:59Z
```

# Should lock file be committed to source control?

---

_@ngaloppo_

Seems like a basic question, but can't find this information in the documentation. 

---

_Comment by @zanieb on 2024-12-11 01:35_

Yep! https://docs.astral.sh/uv/guides/projects/#uvlock and https://docs.astral.sh/uv/concepts/projects/layout/#the-lockfile

---

_Label `question` added by @zanieb on 2024-12-11 02:48_

---

_Comment by @ngaloppo on 2024-12-11 06:10_

Thanks!!

---

_Closed by @ngaloppo on 2024-12-11 06:10_

---

_Comment by @IllarionovDimitri on 2025-09-10 07:54_

@zanieb is the recommendation to commit uv.lock is similar for application and libs? 

from cargo/rust ecosystem i know that:

- Things that produce a final product, namely applications and binaries, should commit Cargo.lock to ensure a deterministic build.
- Library crates should not commit a Cargo.lock file, because it’s irrelevant to any downstream consumers of the library—they will have their own Cargo.lock file;  Cargo.lock file for a library crate is ignored by library users.

what would be your suggestion?

---
