```yaml
number: 2465
title: "Avoid removing un-selected codes when applying `--add-noqa` edits"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/select
created_at: 2023-02-02T03:04:17Z
updated_at: 2023-02-02T03:22:32Z
url: https://github.com/astral-sh/ruff/pull/2465
synced_at: 2026-01-12T04:52:00Z
```

# Avoid removing un-selected codes when applying `--add-noqa` edits

---

_Pull request opened by @charliermarsh on 2023-02-02 03:04_

The downside here is that we have to leave blank `# noqa` directives intact. Otherwise, we risk removing necessary `# noqa` coverage for rules that aren't selected.

Closes #2254.

---

_Renamed from "Respect rule selections when applying --add-noqa" to "Avoid removing un-selected codes when applying `--add-noqa` edits" by @charliermarsh on 2023-02-02 03:04_

---

_Merged by @charliermarsh on 2023-02-02 03:22_

---

_Closed by @charliermarsh on 2023-02-02 03:22_

---

_Branch deleted on 2023-02-02 03:22_

---
