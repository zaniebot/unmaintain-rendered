```yaml
number: 3462
title: Merge user and workspace settings
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/merge
created_at: 2024-05-08T16:55:13Z
updated_at: 2024-05-08T18:49:44Z
url: https://github.com/astral-sh/uv/pull/3462
synced_at: 2026-01-12T16:05:39Z
```

# Merge user and workspace settings

---

_@charliermarsh_

## Summary

This PR follows Cargo's strategy for merging configuration, albeit in a more limited way (we don't support as many configuration locations). Specifically, we merge the user configuration with the workspace configuration if both are present. The workspace configuration has priority, such that we take values from the workspace configuration and ignore those in the user configuration if both are specified for a given setting -- with the exception of arrays and maps, which are concatenated.

For now, if a user provides a configuration file with `--config-file`, we _don't_ merge in the user settings.

See: https://doc.rust-lang.org/cargo/reference/config.html#hierarchical-structure.

Closes #3420.


---

_Label `configuration` added by @charliermarsh on 2024-05-08 16:55_

---

_Review requested from @konstin by @charliermarsh on 2024-05-08 16:56_

---

_Review requested from @zanieb by @charliermarsh on 2024-05-08 16:56_

---

_@zanieb approved on 2024-05-08 18:47_

---

_Merged by @charliermarsh on 2024-05-08 18:49_

---

_Closed by @charliermarsh on 2024-05-08 18:49_

---

_Branch deleted on 2024-05-08 18:49_

---
