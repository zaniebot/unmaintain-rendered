```yaml
number: 10373
title: "Increase recursion limit in `ruff_linter`"
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
base: main
head: zb/recursion
created_at: 2024-03-12T22:41:36Z
updated_at: 2024-03-19T13:15:23Z
url: https://github.com/astral-sh/ruff/pull/10373
synced_at: 2026-01-12T15:55:32Z
```

# Increase recursion limit in `ruff_linter`

---

_@zanieb_

See https://github.com/astral-sh/ruff/pull/10325#issuecomment-1992596557 for motivation. I'm not sure of the implications of the change.

---

_Label `internal` added by @zanieb on 2024-03-12 22:41_

---

_Comment by @charliermarsh on 2024-03-12 22:43_

Huh?!?

---

_Comment by @github-actions[bot] on 2024-03-12 22:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @zanieb on 2024-03-15 16:59_

Something to do with the proc macros... this is beyond me. @snowsignal maybe you could take a look?

---

_Comment by @MichaReiser on 2024-03-18 09:21_

The problem is that the `map_codes` macro creates a very long type signature in 

https://github.com/astral-sh/ruff/blob/e0bc08a7582d64f6e69a69835f748bff59922d6d/crates/ruff_macros/src/map_codes.rs#L390-L397

which is probably also very slow at runtime because we chain a very long sequence of iterators together, meaning that `next` need to check if any iterator still returns an element. 

I think we can rewrite the implementation to use a `Vec` instead. This has the downside that calling `iter` allocates but the method isn't called frequently (to my knowledge).  We potentially could do even better by unrolling `EnumIter` but that's a more involved change

See https://github.com/astral-sh/ruff/pull/10452

---

_Comment by @MichaReiser on 2024-03-19 13:15_

@zanieb I will close this PR because #10452 resolved the underlying issue without increasing the recursion limit.

---

_Closed by @MichaReiser on 2024-03-19 13:15_

---
