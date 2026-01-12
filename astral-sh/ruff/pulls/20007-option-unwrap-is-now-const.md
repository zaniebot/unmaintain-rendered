```yaml
number: 20007
title: "`Option::unwrap` is now const"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/unwrap
created_at: 2025-08-20T17:00:13Z
updated_at: 2025-08-20T17:40:51Z
url: https://github.com/astral-sh/ruff/pull/20007
synced_at: 2026-01-12T15:56:52Z
```

# `Option::unwrap` is now const

---

_@ntBre_

Summary
--

I noticed while working on #20006 that we had a custom `unwrap` function for `Option`. This has been const on stable since 1.83 ([docs](https://doc.rust-lang.org/std/option/enum.Option.html#method.unwrap), [release notes](https://blog.rust-lang.org/2024/11/28/Rust-1.83.0/)), so I think it's safe to use now. I grepped a bit for related todos and found this one for `AsciiCharSet` but no others.

Test Plan
--

Existing tests

---

_Label `internal` added by @ntBre on 2025-08-20 17:00_

---

_@MichaReiser approved on 2025-08-20 17:07_

Nice

---

_Comment by @github-actions[bot] on 2025-08-20 17:14_

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

_Merged by @ntBre on 2025-08-20 17:40_

---

_Closed by @ntBre on 2025-08-20 17:40_

---

_Branch deleted on 2025-08-20 17:40_

---
