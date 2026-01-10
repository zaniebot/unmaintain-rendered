```yaml
number: 6663
title: "Respect `tool.uv.environments` in `pip compile --universal`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/uni
created_at: 2024-08-26T23:41:46Z
updated_at: 2024-08-26T23:58:18Z
url: https://github.com/astral-sh/uv/pull/6663
synced_at: 2026-01-10T13:09:51Z
```

# Respect `tool.uv.environments` in `pip compile --universal`

---

_Pull request opened by @charliermarsh on 2024-08-26 23:41_

## Summary

We now respect the `environments` field in `uv pip compile --universal`, e.g.:

```toml
[tool.uv]
environments = ["platform_system == 'Emscripten'"]
```

Closes https://github.com/astral-sh/uv/issues/6641.


---

_Label `enhancement` added by @charliermarsh on 2024-08-26 23:41_

---

_Merged by @charliermarsh on 2024-08-26 23:58_

---

_Closed by @charliermarsh on 2024-08-26 23:58_

---

_Branch deleted on 2024-08-26 23:58_

---
