```yaml
number: 4529
title: "Remove useless `#[allow(clippy::too_many_arguments)]`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/remove-useless-clippy-too-many-arguments
created_at: 2024-06-25T19:02:51Z
updated_at: 2024-06-25T19:10:00Z
url: https://github.com/astral-sh/uv/pull/4529
synced_at: 2026-01-12T16:06:17Z
```

# Remove useless `#[allow(clippy::too_many_arguments)]`

---

_@konstin_

I went through all `#[allow(clippy::too_many_arguments)]` and removed the useless ones.

---

_Label `internal` added by @konstin on 2024-06-25 19:02_

---

_Comment by @charliermarsh on 2024-06-25 19:04_

Sounds good. Should we disable this and too-many-bools perhaps?

---

_@BurntSushi approved on 2024-06-25 19:05_

Nice!

Although I'd still be in favor of just turning this off. If I have a function with too many parameters that Clippy yells at me, I don't think that's going to cause me to do anything other than `allow` the lint for that function. ¯\\\_(ツ)_/¯

---

_Merged by @konstin on 2024-06-25 19:09_

---

_Closed by @konstin on 2024-06-25 19:09_

---

_Branch deleted on 2024-06-25 19:10_

---
