```yaml
number: 3412
title: "Preserve given for `tool.uv.sources` paths"
type: pull_request
state: merged
author: konstin
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: konsti/sources-preserve-path
created_at: 2024-05-06T16:46:01Z
updated_at: 2024-05-07T09:00:03Z
url: https://github.com/astral-sh/uv/pull/3412
synced_at: 2026-01-12T16:05:37Z
```

# Preserve given for `tool.uv.sources` paths

---

_@konstin_

We now correctly emit relative paths in `uv pip compile` with `tool.uv.sources` path inputs.

`tool.uv.sources` is mainly intended to be used with the uv lock file over requirements.txt, but it's good to have basic `uv pip` support working.

Fixes #3366


---

_Label `preview` added by @konstin on 2024-05-06 16:46_

---

_Review requested from @charliermarsh by @konstin on 2024-05-06 16:46_

---

_@konstin reviewed on 2024-05-06 16:46_

---

_Review comment by @konstin on `crates/pep508-rs/src/verbatim_url.rs`:98 on 2024-05-06 16:46_

@charliermarsh Do you know why this used to be `None`?


---

_Review comment by @charliermarsh on `crates/pep508-rs/src/verbatim_url.rs`:98 on 2024-05-06 17:13_

Callers are expected to set this using `with_given`, can we do that instead?

---

_@charliermarsh reviewed on 2024-05-06 17:13_

---

_@charliermarsh approved on 2024-05-06 18:15_

---

_Label `bug` added by @charliermarsh on 2024-05-06 18:15_

---

_Merged by @charliermarsh on 2024-05-07 09:00_

---

_Closed by @charliermarsh on 2024-05-07 09:00_

---

_Branch deleted on 2024-05-07 09:00_

---
