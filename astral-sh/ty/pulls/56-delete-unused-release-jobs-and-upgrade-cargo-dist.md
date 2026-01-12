```yaml
number: 56
title: Delete unused release jobs and upgrade cargo-dist
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: delete-unused-release-jobs
created_at: 2025-05-06T06:24:08Z
updated_at: 2025-05-06T14:02:26Z
url: https://github.com/astral-sh/ty/pull/56
synced_at: 2026-01-12T15:54:27Z
```

# Delete unused release jobs and upgrade cargo-dist

---

_@MichaReiser_

* `notify-dependents`: We don't have a pre-commit hook (and I don't think we want one at the moment)
* `publish-playground`: We want to keep publishing new versions with every PR, at least for the short term and that's easier in the ty repo
* `publish-docs`: We have no docs
* `publish-wasm`: I don't want to publish a wasm build just yet


---

_Review requested from @Gankra by @MichaReiser on 2025-05-06 06:24_

---

_Label `release` added by @MichaReiser on 2025-05-06 12:04_

---

_Closed by @MichaReiser on 2025-05-06 12:04_

---

_Reopened by @MichaReiser on 2025-05-06 12:04_

---

_@zanieb approved on 2025-05-06 12:19_

---

_Review requested from @zanieb by @MichaReiser on 2025-05-06 12:45_

---

_Renamed from "Delete unused release jobs" to "Delete unused release jobs and upgrade cargo-dist" by @zanieb on 2025-05-06 12:49_

---

_@zanieb reviewed on 2025-05-06 12:49_

---

_Review comment by @zanieb on `dist-workspace.toml`:21 on 2025-05-06 12:49_

Can you reformat this list? Can a formatter do that for us? I do it manually in uv.

---

_@zanieb approved on 2025-05-06 12:49_

---

_Comment by @zanieb on 2025-05-06 12:50_

Reverts https://github.com/astral-sh/ty/pull/53 and https://github.com/astral-sh/ty/pull/51

---

_Merged by @MichaReiser on 2025-05-06 12:52_

---

_Closed by @MichaReiser on 2025-05-06 12:52_

---

_@Gankra reviewed on 2025-05-06 14:02_

nice

---
