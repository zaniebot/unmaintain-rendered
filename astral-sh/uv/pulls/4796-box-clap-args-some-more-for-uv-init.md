```yaml
number: 4796
title: "Box clap args some more for `uv init`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/box-some-more
created_at: 2024-07-04T07:48:10Z
updated_at: 2024-07-04T17:22:25Z
url: https://github.com/astral-sh/uv/pull/4796
synced_at: 2026-01-12T16:06:28Z
```

# Box clap args some more for `uv init`

---

_@konstin_

Fixes stack overflows in tests for https://github.com/astral-sh/uv/pull/4791

---

_Label `internal` added by @konstin on 2024-07-04 07:48_

---

_Comment by @konstin on 2024-07-04 07:56_

There is one failure due to portable paths missing for the toml snapshots otherwise the tests are now passing.

---

_Marked ready for review by @konstin on 2024-07-04 07:56_

---

_Review requested from @ibraheemdev by @konstin on 2024-07-04 07:56_

---

_Comment by @charliermarsh on 2024-07-04 17:09_

I'm going to rebase this on `main` and merge, to unblock the rest of the PRs... Right now we can't seem to merge project changes.

---

_Merged by @charliermarsh on 2024-07-04 17:22_

---

_Closed by @charliermarsh on 2024-07-04 17:22_

---

_Branch deleted on 2024-07-04 17:22_

---
