```yaml
number: 14757
title: Move dependency group normalization into specification
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/group-validation
created_at: 2025-07-20T17:55:31Z
updated_at: 2025-07-20T18:13:28Z
url: https://github.com/astral-sh/uv/pull/14757
synced_at: 2026-01-12T16:11:23Z
```

# Move dependency group normalization into specification

---

_@charliermarsh_

## Summary

A refactor that I'm extracting from #14755. There should be no functional changes, but the core idea is to postpone filling in the default `path` for a dependency group until we make the specification. This allows us to use the groups for the `pylock.toml` in the future, if such a `pylock.toml` is provided.


---

_Label `internal` added by @charliermarsh on 2025-07-20 17:55_

---

_Marked ready for review by @charliermarsh on 2025-07-20 17:55_

---

_Merged by @charliermarsh on 2025-07-20 18:13_

---

_Closed by @charliermarsh on 2025-07-20 18:13_

---

_Branch deleted on 2025-07-20 18:13_

---
