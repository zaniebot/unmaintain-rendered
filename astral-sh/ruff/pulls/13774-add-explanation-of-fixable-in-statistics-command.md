```yaml
number: 13774
title: "Add explanation of fixable in `--statistics` command"
type: pull_request
state: merged
author: sbrugman
labels:
  - cli
assignees: []
merged: true
base: main
head: statistics-fixable-help
created_at: 2024-10-16T10:59:13Z
updated_at: 2024-10-17T09:15:16Z
url: https://github.com/astral-sh/ruff/pull/13774
synced_at: 2026-01-12T15:55:45Z
```

# Add explanation of fixable in `--statistics` command

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix for #13717.
See issue for discussion.

## Test Plan

Updated integration tests

---

_Comment by @github-actions[bot] on 2024-10-16 11:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `cli` added by @MichaReiser on 2024-10-16 14:19_

---

_Review comment by @MichaReiser on `crates/ruff/src/printer.rs`:424 on 2024-10-16 14:20_

I would use something closer to the `ruff check` message
```suggestion
                        "[*] fixable with `ruff check --fix`
```

Original message:

```
[*] 30 fixable with the `--fix` option (31 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

---

_@MichaReiser reviewed on 2024-10-16 14:20_

---

_Comment by @MichaReiser on 2024-10-16 14:21_

Hi @sbrugman It finally clicked. I obviously remember your contributions! 

---

_@MichaReiser approved on 2024-10-17 06:01_

Thanks

---

_Merged by @MichaReiser on 2024-10-17 06:02_

---

_Closed by @MichaReiser on 2024-10-17 06:02_

---

_Branch deleted on 2024-10-17 09:15_

---
