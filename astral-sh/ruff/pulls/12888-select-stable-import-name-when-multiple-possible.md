```yaml
number: 12888
title: Select stable import name when multiple possible bindings are in scope
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: stable-import-fix
created_at: 2024-08-14T09:41:54Z
updated_at: 2024-08-16T18:16:59Z
url: https://github.com/astral-sh/ruff/pull/12888
synced_at: 2026-01-10T21:38:32Z
```

# Select stable import name when multiple possible bindings are in scope

---

_Pull request opened by @MichaReiser on 2024-08-14 09:41_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/12885

The issue with the `FURB177` test is that there are two viable bindings for `pathlib.Path`: `pathlib.Path` or `Path` but `bindings` is a `HashMap` that has no guaranteed iteration order. 

I'm reluctant to change `bindings` to a `BTreeMap`. I would suspect that it comes with quiet a performance penalty. That's why this PR fixes this issue locally 
by sorting the imports by their range and picking the last import if multiple options exist. 


## Test Plan

`cargo test`

## Alternatives

Split the test into two files. One that uses `import pathlib` and one with `from pathlib import Path`


---

_Label `internal` added by @MichaReiser on 2024-08-14 09:42_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-08-14 09:42_

---

_Renamed from "Select stable import name" to "Select stable import name when multiple possible bindings are in scope" by @MichaReiser on 2024-08-14 09:49_

---

_Comment by @github-actions[bot] on 2024-08-14 09:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @WhyNotHugo on 2024-08-14 12:34_

Tests pass on 32bit architectures: https://gitlab.alpinelinux.org/WhyNotHugo/aports/-/pipelines/252939

Some of the more exotic architectures are not done, but this does fix https://github.com/astral-sh/ruff/issues/12885

---

_Comment by @MichaReiser on 2024-08-16 15:24_

@charliermarsh any thoughts on this?

---

_@charliermarsh approved on 2024-08-16 16:36_

Thanks!

---

_Merged by @MichaReiser on 2024-08-16 18:16_

---

_Closed by @MichaReiser on 2024-08-16 18:16_

---

_Branch deleted on 2024-08-16 18:16_

---
