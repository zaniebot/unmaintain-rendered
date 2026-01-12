```yaml
number: 955
title: Allow preservation of external check codes
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/external
created_at: 2022-11-29T03:15:01Z
updated_at: 2022-11-29T03:16:19Z
url: https://github.com/astral-sh/ruff/pull/955
synced_at: 2026-01-12T05:48:46Z
```

# Allow preservation of external check codes

---

_Pull request opened by @charliermarsh on 2022-11-29 03:15_

Users can now specify a field like:

```toml
[tool.ruff]
external = ["V101"]
```

Under this configuration, `V101` would be ignored when checking `M001` errors, and when adding `# noqa` directives with `--add-noqa`.

Resolves #951.


---

_Merged by @charliermarsh on 2022-11-29 03:16_

---

_Closed by @charliermarsh on 2022-11-29 03:16_

---

_Branch deleted on 2022-11-29 03:16_

---
