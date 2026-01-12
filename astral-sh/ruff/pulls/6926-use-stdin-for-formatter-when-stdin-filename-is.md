```yaml
number: 6926
title: "Use stdin for formatter when `--stdin-filename` is provided"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/format-cli
created_at: 2023-08-27T20:08:26Z
updated_at: 2023-08-27T20:32:34Z
url: https://github.com/astral-sh/ruff/pull/6926
synced_at: 2026-01-12T15:55:22Z
```

# Use stdin for formatter when `--stdin-filename` is provided

---

_@charliermarsh_

## Summary

Just making the formatter CLI more consistent with the linter -- e.g., we now use stdin on invocations like `cat foo.py | cargo run -p ruff_cli -- format -- --stdin-filename=foo.py`, instead of _only_ relying on the `-` file (and use the same helper as the linter to facilitate this).

---

_Renamed from "Use stdin for formatter when --stdin-filename is provided" to "Use stdin for formatter when `--stdin-filename` is provided" by @charliermarsh on 2023-08-27 20:08_

---

_Label `formatter` added by @charliermarsh on 2023-08-27 20:08_

---

_Merged by @charliermarsh on 2023-08-27 20:32_

---

_Closed by @charliermarsh on 2023-08-27 20:32_

---

_Branch deleted on 2023-08-27 20:32_

---

_Comment by @github-actions[bot] on 2023-08-27 20:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---
