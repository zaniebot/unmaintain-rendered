```yaml
number: 11283
title: Expand tildes when resolving Ruff server configuration file
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: charlie/expand
created_at: 2024-05-04T17:31:51Z
updated_at: 2024-05-04T17:51:27Z
url: https://github.com/astral-sh/ruff/pull/11283
synced_at: 2026-01-12T15:55:37Z
```

# Expand tildes when resolving Ruff server configuration file

---

_@charliermarsh_

## Summary

Users can now include tildes and environment variables in the provided path, just like with `--config`.

Closes #11277.

## Test Plan

Set the configuration path to `"ruff.configuration": "~/x.toml"`; verified that the server attempted to read from `/Users/crmarsh/x.toml`.

![Screenshot 2024-05-04 at 1 31 43 PM](https://github.com/astral-sh/ruff/assets/1309177/ea9829cd-6d8a-4818-a47c-dcff9219e996)


---

_Label `bug` added by @charliermarsh on 2024-05-04 17:32_

---

_Label `server` added by @charliermarsh on 2024-05-04 17:32_

---

_Review requested from @snowsignal by @charliermarsh on 2024-05-04 17:32_

---

_@charliermarsh reviewed on 2024-05-04 17:32_

---

_Review comment by @charliermarsh on `crates/ruff_server/src/session/settings.rs`:239 on 2024-05-04 17:32_

Is there a specific reason that we needed `OsString::from` then `into()` here before? Want to make sure I'm not breaking anything.

---

_@snowsignal reviewed on 2024-05-04 17:48_

---

_Review comment by @snowsignal on `crates/ruff_server/src/session/settings.rs`:239 on 2024-05-04 17:48_

No, that was just a conversion from `String` to `PathBuf` 🙂 

---

_@snowsignal approved on 2024-05-04 17:49_

---

_Comment by @github-actions[bot] on 2024-05-04 17:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @charliermarsh on 2024-05-04 17:51_

---

_Closed by @charliermarsh on 2024-05-04 17:51_

---

_Branch deleted on 2024-05-04 17:51_

---
