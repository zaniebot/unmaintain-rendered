```yaml
number: 3618
title: Apply combination logic to merge CLI and persistent configuration
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/combine-cli
created_at: 2024-05-15T18:59:10Z
updated_at: 2024-05-20T13:37:03Z
url: https://github.com/astral-sh/uv/pull/3618
synced_at: 2026-01-10T14:32:20Z
```

# Apply combination logic to merge CLI and persistent configuration

---

_Pull request opened by @charliermarsh on 2024-05-15 18:59_

## Summary

If you have (e.g.) `extra-index-url` in your configuration file _and_ provide `--extra-index-url` on the command-line, we now merge the options rather than ignoring those in the configuration file. As such, merging the CLI and the persistent configuration is now semantically identical to how we merge (project persistent configuration) with (user persistent configuration).

Closes https://github.com/astral-sh/uv/issues/3541.


---

_Review requested from @zanieb by @charliermarsh on 2024-05-15 18:59_

---

_Label `configuration` added by @charliermarsh on 2024-05-15 18:59_

---

_Marked ready for review by @charliermarsh on 2024-05-15 18:59_

---

_@charliermarsh reviewed on 2024-05-15 19:05_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:967 on 2024-05-15 19:05_

The changes here are exclusively replacing `.or` with `.combine`.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-20 01:56_

---

_@BurntSushi approved on 2024-05-20 12:12_

Seems reasonable to me!

---

_@zanieb approved on 2024-05-20 12:42_

---

_Merged by @charliermarsh on 2024-05-20 13:37_

---

_Closed by @charliermarsh on 2024-05-20 13:37_

---

_Branch deleted on 2024-05-20 13:37_

---
