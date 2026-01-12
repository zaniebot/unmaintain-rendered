```yaml
number: 3648
title: "Replace `logical_lines` feature with `debug_assertions`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/debug-flag
created_at: 2023-03-21T15:18:01Z
updated_at: 2023-03-21T16:16:43Z
url: https://github.com/astral-sh/ruff/pull/3648
synced_at: 2026-01-12T04:39:45Z
```

# Replace `logical_lines` feature with `debug_assertions`

---

_Pull request opened by @charliermarsh on 2023-03-21 15:18_

See: https://github.com/charliermarsh/ruff/pull/3557#issuecomment-1473352587.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-21 15:18_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/tests/api.rs`:26 on 2023-03-21 15:54_

Are the changes in this file intentional?

---

_@MichaReiser approved on 2023-03-21 15:54_

---

_Renamed from "Replace `logical_lines` flag with `debug_assertions`" to "Replace `logical_lines` feature with `debug_assertions`" by @MichaReiser on 2023-03-21 15:56_

---

_@charliermarsh reviewed on 2023-03-21 15:59_

---

_Review comment by @charliermarsh on `crates/ruff_wasm/tests/api.rs`:26 on 2023-03-21 15:59_

Yeah, because these rules are now enabled in debug here (whereas before, I don't think we were running with `--all-features`). So without these changes, we get additional pycodestyle errors.

---

_Merged by @charliermarsh on 2023-03-21 16:16_

---

_Closed by @charliermarsh on 2023-03-21 16:16_

---

_Branch deleted on 2023-03-21 16:16_

---
