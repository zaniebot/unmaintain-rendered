```yaml
number: 9522
title: Make pip install snapshot independent of settings fields
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/unknown-field-errpr
created_at: 2024-11-29T13:31:15Z
updated_at: 2024-11-29T17:32:50Z
url: https://github.com/astral-sh/uv/pull/9522
synced_at: 2026-01-12T16:08:50Z
```

# Make pip install snapshot independent of settings fields

---

_@konstin_

When changing something about the settings, `invalid_pyproject_toml_option_unknown_field` would fail unexpectedly because the exact list of possible options had changed. Since we're already testing this list in the settings-related test `resolve_config_file`, i'm stubbing the exact output here.

---

_Label `internal` added by @konstin on 2024-11-29 13:31_

---

_Review requested from @charliermarsh by @konstin on 2024-11-29 13:31_

---

_@charliermarsh approved on 2024-11-29 17:05_

---

_Merged by @charliermarsh on 2024-11-29 17:32_

---

_Closed by @charliermarsh on 2024-11-29 17:32_

---

_Branch deleted on 2024-11-29 17:32_

---
