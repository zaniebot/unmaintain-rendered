```yaml
number: 10170
title: "Remove deprecated CLI option `--format`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - breaking
  - cli
assignees: []
merged: true
base: main
head: remove-format-cli-option
created_at: 2024-02-29T13:52:07Z
updated_at: 2024-02-29T14:05:11Z
url: https://github.com/astral-sh/ruff/pull/10170
synced_at: 2026-01-12T15:55:31Z
```

# Remove deprecated CLI option `--format`

---

_@MichaReiser_

## Summary

Extracted from https://github.com/astral-sh/ruff/pull/9814

Removes the deprecated CLI option 'format' in favor of `output-format`. The option was deprecated in https://github.com/astral-sh/ruff/pull/8203



## Test Plan

```
cargo run -p ruff -- --explain RET505 #  works
cargo run -p ruff -- --explain RET505 --output-format json # works
cargo run -p ruff -- --explain RET505 --format json # does not work anymore
error: unexpected argument '--format' found

  tip: to pass '--format' as a value, use '-- --format'

```

---

_Added to milestone `v0.3.0` by @MichaReiser on 2024-02-29 13:52_

---

_Label `breaking` added by @MichaReiser on 2024-02-29 13:53_

---

_Label `cli` added by @MichaReiser on 2024-02-29 13:53_

---

_Marked ready for review by @MichaReiser on 2024-02-29 13:54_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-29 13:55_

---

_Comment by @MichaReiser on 2024-02-29 13:57_

Thanks  @tibor-reiss 

---

_Merged by @MichaReiser on 2024-02-29 13:59_

---

_Closed by @MichaReiser on 2024-02-29 13:59_

---

_Branch deleted on 2024-02-29 13:59_

---

_Comment by @github-actions[bot] on 2024-02-29 14:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
