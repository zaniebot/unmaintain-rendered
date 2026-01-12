```yaml
number: 8803
title: "Publish: Hint at `--skip-existing` -> `--check-url` transition"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: konsti/hint-check-url
created_at: 2024-11-04T09:36:36Z
updated_at: 2024-11-04T23:21:02Z
url: https://github.com/astral-sh/uv/pull/8803
synced_at: 2026-01-12T16:08:30Z
```

# Publish: Hint at `--skip-existing` -> `--check-url` transition

---

_@konstin_

See https://github.com/astral-sh/uv/pull/8531#issuecomment-2442698889, we hint users coming from twine to use `--check-url` instead.

> `uv publish` does not support `--skip-existing`, use `--check-url` with the simple index URL instead.

---

_Label `enhancement` added by @konstin on 2024-11-04 09:36_

---

_Label `preview` added by @konstin on 2024-11-04 09:36_

---

_Review requested from @zanieb by @konstin on 2024-11-04 09:36_

---

_@zanieb reviewed on 2024-11-04 12:23_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1134 on 2024-11-04 12:23_

Can you give users a little more context here?

---

_@zanieb reviewed on 2024-11-04 12:23_

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1134 on 2024-11-04 12:23_

(I think this will be fairly common to encounter and it seems worth explaining why the API is different)

---

_@konstin reviewed on 2024-11-04 12:39_

---

_Review comment by @konstin on `crates/uv/src/lib.rs`:1134 on 2024-11-04 12:39_

Like what sort of context? Explaining why `--skip-existing` is broken?

---

_Review comment by @zanieb on `crates/uv/src/lib.rs`:1134 on 2024-11-04 12:47_

Yeah like

> `uv publish` does not support `--skip-existing` because there is not a reliable way to identify when an upload fails due to an existing distribution. Instead, use `--check-url` to provide the URL to the simple API for your index. uv will check the index for existing distributions before attempting uploads.

---

_@zanieb reviewed on 2024-11-04 12:47_

---

_@zanieb approved on 2024-11-04 23:09_

---

_Merged by @zanieb on 2024-11-04 23:21_

---

_Closed by @zanieb on 2024-11-04 23:21_

---

_Branch deleted on 2024-11-04 23:21_

---
