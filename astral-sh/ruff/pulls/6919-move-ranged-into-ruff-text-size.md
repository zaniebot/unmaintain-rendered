```yaml
number: 6919
title: "Move `Ranged` into `ruff_text_size`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/move
created_at: 2023-08-27T15:49:39Z
updated_at: 2023-08-27T18:12:52Z
url: https://github.com/astral-sh/ruff/pull/6919
synced_at: 2026-01-12T15:55:22Z
```

# Move `Ranged` into `ruff_text_size`

---

_@charliermarsh_


## Summary

The motivation here is that this enables us to implement `Ranged` in crates that don't depend on `ruff_python_ast`.

Largely a mechanical refactor with a lot of regex, Clippy help, and manual fixups.

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-27 15:49_

---

_Label `internal` added by @charliermarsh on 2023-08-27 15:49_

---

_Converted to draft by @charliermarsh on 2023-08-27 16:00_

---

_Comment by @charliermarsh on 2023-08-27 16:03_

Trying to fix these failures but my `git push` is hanging 😅 

---

_Marked ready for review by @charliermarsh on 2023-08-27 16:30_

---

_Comment by @MichaReiser on 2023-08-27 16:39_

Oh god... I hope this is not clashing with my refactor xD

---

_@MichaReiser approved on 2023-08-27 16:39_

---

_Comment by @github-actions[bot] on 2023-08-27 16:42_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-08-27 17:45_

Oh god. Do you want me to do the rebase?

---

_Comment by @MichaReiser on 2023-08-27 18:10_

> Oh god. Do you want me to do the rebase?

Go ahead, we should be fine for most files :)

---

_Merged by @charliermarsh on 2023-08-27 18:12_

---

_Closed by @charliermarsh on 2023-08-27 18:12_

---

_Branch deleted on 2023-08-27 18:12_

---
