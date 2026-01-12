```yaml
number: 184
title: "chore: Avoid `collect` in `inner_main`"
type: pull_request
state: merged
author: Stranger6667
labels: []
assignees: []
merged: true
base: main
head: dd/avoid-cast
created_at: 2022-09-14T08:41:52Z
updated_at: 2022-09-14T12:16:05Z
url: https://github.com/astral-sh/ruff/pull/184
synced_at: 2026-01-12T05:48:45Z
```

# chore: Avoid `collect` in `inner_main`

---

_Pull request opened by @Stranger6667 on 2022-09-14 08:41_

I tried to guess if it was what you meant by the `todo` entry. With this change, there is a bit less monomorphization & no vector allocation. Additionally `load_config` has a simpler signature without `Result` (as one of the `clippy` lints suggests) + similarly `Settings::from_paths`. Let me know what you think :)

---

_Comment by @charliermarsh on 2022-09-14 12:15_

This is great! Thank you! That _was_ the intent.

---

_Merged by @charliermarsh on 2022-09-14 12:16_

---

_Closed by @charliermarsh on 2022-09-14 12:16_

---
