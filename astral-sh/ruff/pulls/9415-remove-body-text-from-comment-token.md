```yaml
number: 9415
title: Remove body text from comment token
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
base: main
head: charlie/comment
created_at: 2024-01-06T21:05:55Z
updated_at: 2024-01-08T14:43:59Z
url: https://github.com/astral-sh/ruff/pull/9415
synced_at: 2026-01-12T15:55:28Z
```

# Remove body text from comment token

---

_@charliermarsh_

## Summary

Remarkably, we don't use this at all. I wonder if this will show up in benchmarks?

---

_Label `performance` added by @charliermarsh on 2024-01-06 21:06_

---

_Comment by @github-actions[bot] on 2024-01-06 21:38_

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

_Review requested from @MichaReiser by @charliermarsh on 2024-01-07 17:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/token.rs`:271 on 2024-01-08 08:00_

I'm worried that emitting `Comment` here is confusing. I'm not sure what a better value is; maybe I'm making it clear that it is a placeholder `<Comment>` and not the actual `Comment`?

---

_@MichaReiser approved on 2024-01-08 08:01_

I'm leaning towards leaving it as is for the below reasons, but leaving it up to you

This change doesn't seem to improve performance and it makes `Comment` the odd one of our tokens, because it is the only one that doesn't store its own value. 

I think removing the value only helps when it helps to reduce the size of `Tok` or when it removes the need for the automatic `Drop` implementation of `Tok`. However, this only becomes possible once we remove all allocated values from Token (`Name`, `String`, `FStringMiddle`, `IpyEscapeCommand`, `Int`), which is a bit more involved. 



---

_Comment by @charliermarsh on 2024-01-08 14:43_

I will close for now, but I'm likely going to revive this once we have the new parser.

---

_Closed by @charliermarsh on 2024-01-08 14:43_

---
