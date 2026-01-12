```yaml
number: 1929
title: "Define origin names & URLs within doc comments"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: origin-doc-comments
created_at: 2023-01-17T05:44:04Z
updated_at: 2023-01-17T12:44:41Z
url: https://github.com/astral-sh/ruff/pull/1929
synced_at: 2026-01-12T15:55:07Z
```

# Define origin names & URLs within doc comments

---

_@not-my-profile_

_No description provided._

---

_Review comment by @charliermarsh on `build.rs`:50 on 2023-01-17 12:22_

Let's error (for now) if the URL isn't `https://github `or `https://pypi`.

---

_Review comment by @charliermarsh on `ruff_dev/src/generate_rules_table.rs`:75 on 2023-01-17 12:23_

Similarly, let's error on this branch for now.

---

_Review comment by @charliermarsh on `build.rs`:81 on 2023-01-17 12:25_

If these are only being used in the generation script, do we need to inject them into the program? Could we not put this logic directly in the generation script?

---

_@charliermarsh reviewed on 2023-01-17 12:25_

---

_@not-my-profile reviewed on 2023-01-17 12:34_

---

_Review comment by @not-my-profile on `build.rs`:81 on 2023-01-17 12:34_

It's not only used by `ruff_dev` since `name()` is also used by the `--explain` command of `ruff_cli`.

---

_@not-my-profile reviewed on 2023-01-17 12:41_

---

_Review comment by @not-my-profile on `build.rs`:50 on 2023-01-17 12:41_

I have updated `generate_rules_table.rs` to error for other URLs ... since that is already running in the CI, we don't have to check for it in `build.rs`.

---

_@not-my-profile reviewed on 2023-01-17 12:41_

---

_Review comment by @not-my-profile on `ruff_dev/src/generate_rules_table.rs`:75 on 2023-01-17 12:41_

Done.

---

_@charliermarsh reviewed on 2023-01-17 12:42_

---

_Review comment by @charliermarsh on `build.rs`:81 on 2023-01-17 12:42_

Ahh right, ok!

---

_Merged by @charliermarsh on 2023-01-17 12:44_

---

_Closed by @charliermarsh on 2023-01-17 12:44_

---
