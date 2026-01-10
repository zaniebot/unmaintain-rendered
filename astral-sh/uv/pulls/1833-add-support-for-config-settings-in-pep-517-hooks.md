```yaml
number: 1833
title: "Add support for `config_settings` in PEP 517 hooks"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/config-setting
created_at: 2024-02-21T21:32:47Z
updated_at: 2024-02-23T00:53:46Z
url: https://github.com/astral-sh/uv/pull/1833
synced_at: 2026-01-10T14:54:43Z
```

# Add support for `config_settings` in PEP 517 hooks

---

_Pull request opened by @charliermarsh on 2024-02-21 21:32_

## Summary

Adds `--config-setting` / `-C` (with a `--config-settings` alias for convenience) to the CLI.

Closes https://github.com/astral-sh/uv/issues/1460.


---

_Label `enhancement` added by @charliermarsh on 2024-02-21 21:32_

---

_Label `compatibility` added by @charliermarsh on 2024-02-21 21:32_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-21 22:59_

---

_Marked ready for review by @charliermarsh on 2024-02-21 23:00_

---

_Review comment by @konstin on `crates/uv-traits/src/lib.rs`:359 on 2024-02-22 09:10_

Since it's all strings that should be the mutual subset of json and python, can we have the escaping done by serde?

---

_@konstin approved on 2024-02-22 09:14_

---

_Merged by @charliermarsh on 2024-02-23 00:53_

---

_Closed by @charliermarsh on 2024-02-23 00:53_

---

_Branch deleted on 2024-02-23 00:53_

---
