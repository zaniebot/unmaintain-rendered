```yaml
number: 17492
title: "[syntax-error] from __future__ import braces "
type: pull_request
state: closed
author: abhijeetbodas2001
labels: []
assignees: []
base: main
head: sysexit
created_at: 2025-04-20T08:42:00Z
updated_at: 2025-04-22T03:37:20Z
url: https://github.com/astral-sh/ruff/pull/17492
synced_at: 2026-01-10T19:33:02Z
```

# [syntax-error] from __future__ import braces 

---

_Pull request opened by @abhijeetbodas2001 on 2025-04-20 08:42_

An easter egg -- trying to import this gives a "not a chance" syntax
error.

Part of: #17412 


---

_Review requested from @MichaReiser by @abhijeetbodas2001 on 2025-04-20 08:42_

---

_Review requested from @dhruvmanila by @abhijeetbodas2001 on 2025-04-20 08:42_

---

_@abhijeetbodas2001 reviewed on 2025-04-20 08:42_

---

_Review comment by @abhijeetbodas2001 on `crates/ruff_python_stdlib/src/future.rs`:17 on 2025-04-20 08:42_

Until syntax errors stabilize, this can be considered a regression.
Happy to drop this commit if appropriate.

---

_Comment by @abhijeetbodas2001 on 2025-04-20 08:43_

@ntBre could you review this?

---

_Comment by @github-actions[bot] on 2025-04-20 10:53_

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

_Comment by @dhruvmanila on 2025-04-21 16:35_

I'd prefer to not include this in Ruff as it's mainly an easter egg for the CPython parser and not really a syntax error. Neither Pyright or mypy raises this as a syntax error.

---

_Comment by @ntBre on 2025-04-21 16:44_

Ah, that makes sense to me. My mistake including it in the issue.

@abhijeetbodas2001 thank you for working on this, but I think we should close this for now.

---

_Closed by @ntBre on 2025-04-21 16:44_

---

_Comment by @abhijeetbodas2001 on 2025-04-22 03:37_

Got it. I'll pick up another syntax error to work on then. Thanks!

---

_Branch deleted on 2025-04-22 03:37_

---
