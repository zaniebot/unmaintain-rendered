```yaml
number: 8596
title: Write unchanged, excluded files to stdout when read via stdin
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/parrot
created_at: 2023-11-10T03:49:16Z
updated_at: 2023-11-10T04:15:15Z
url: https://github.com/astral-sh/ruff/pull/8596
synced_at: 2026-01-12T15:55:26Z
```

# Write unchanged, excluded files to stdout when read via stdin

---

_@charliermarsh_

## Summary

When you run Ruff via stdin, and pass `format` or `check --fix`, we typically write the changed or unchanged contents to stdout. It turns out we forgot to do this when the file is _excluded_, so if you run `ruff format /path/to/excluded/file.py`, we don't write _anything_ to `stdout`. This led to a bug in the LSP whereby we deleted file contents for third-party files.

The right thing to do here is write back the unchanged contents, as it should always be safe to write the output of stdout back to a file.


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-11-10 03:49_

---

_Label `bug` added by @charliermarsh on 2023-11-10 03:49_

---

_Label `cli` added by @charliermarsh on 2023-11-10 03:49_

---

_Comment by @charliermarsh on 2023-11-10 03:55_

Once we make this change, we can in turn change the LSP to always write empty fixes on supported Ruff versions.

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/stdin.rs`:12 on 2023-11-10 04:10_

Nice name!

---

_Comment by @github-actions[bot] on 2023-11-10 04:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2023-11-10 04:14_

---

_Merged by @charliermarsh on 2023-11-10 04:15_

---

_Closed by @charliermarsh on 2023-11-10 04:15_

---

_Branch deleted on 2023-11-10 04:15_

---

_@charliermarsh reviewed on 2023-11-10 04:15_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/stdin.rs`:12 on 2023-11-10 04:15_

Haha I wasn't sure if it would be clear but I'm glad :)

---
