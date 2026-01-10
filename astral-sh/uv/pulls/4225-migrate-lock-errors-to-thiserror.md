```yaml
number: 4225
title: "Migrate lock errors to `thiserror`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/thiserror
created_at: 2024-06-10T23:30:04Z
updated_at: 2024-06-11T11:53:44Z
url: https://github.com/astral-sh/uv/pull/4225
synced_at: 2026-01-10T13:54:02Z
```

# Migrate lock errors to `thiserror`

---

_Pull request opened by @charliermarsh on 2024-06-10 23:30_

## Summary

Do we prefer this?



---

_Label `internal` added by @charliermarsh on 2024-06-10 23:36_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-10 23:37_

---

_Review requested from @konstin by @charliermarsh on 2024-06-10 23:37_

---

_@konstin approved on 2024-06-11 08:04_

Yes!

---

_@BurntSushi approved on 2024-06-11 11:14_

I don't have strong feelings either way.

I've also been wondering if we should just switch to `anyhow::Error` (or similar) for most of our error handling. I'm not sure we really make use of our structured errors much. (And `anyhow::Error` doesn't preclude it, although it makes it a little more cumbersome.)

---

_Comment by @konstin on 2024-06-11 11:35_

This is my experience from comparing maturin (anyhow everywhere) to uv (mostly thiserror): I like thiserror for making it easy to trace through the error chain, we ensure we're neither missing cases nor adding repetitive context and it makes the kinds of errors that can occur visible. `#[error()]` # `#[from]` removes a bunch of repetition that you get when adding `.context` yourself and moves the error formatting away from the business logic to a dedicated error type (You only add `?` vs. a `.context()` or even a `.with_context()` that breaks your line). 

---

_Merged by @charliermarsh on 2024-06-11 11:40_

---

_Closed by @charliermarsh on 2024-06-11 11:40_

---

_Branch deleted on 2024-06-11 11:40_

---

_Comment by @charliermarsh on 2024-06-11 11:53_

I think my experience is similar: I actually prefer thiserror for uv because it forces us to think about and track the variants and how they’re used.

---
