```yaml
number: 16984
title: "[syntax-errors] Store to or delete `__debug__`"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/del-debug-3.10
created_at: 2025-03-26T17:24:10Z
updated_at: 2025-03-29T16:07:22Z
url: https://github.com/astral-sh/ruff/pull/16984
synced_at: 2026-01-12T15:55:59Z
```

# [syntax-errors] Store to or delete `__debug__`

---

_@ntBre_

Summary
--

Detect setting or deleting `__debug__`. Assigning to `__debug__` was a `SyntaxError` on the earliest version I tested (3.8). Deleting `__debug__` was made a `SyntaxError` in [BPO 45000], which said it was resolved in Python 3.10. However, `del __debug__` was also a runtime error (`NameError`) when I tested in Python 3.9.6, so I thought it was worth including 3.9 in this check.

I don't think it was ever a *good* idea to try `del __debug__`, so I think there's also an argument for not making this version-dependent at all. That would only simplify the implementation very slightly, though.

[BPO 45000]: https://github.com/python/cpython/issues/89163

Test Plan
--

New inline tests. This also required adding a `PythonVersion` field to the `TestContext` that could be taken from the inline `ParseOptions` and making the version field on the options accessible.

---

_Label `rule` added by @ntBre on 2025-03-26 17:24_

---

_Label `preview` added by @ntBre on 2025-03-26 17:24_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-26 17:24_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-26 17:24_

---

_Comment by @github-actions[bot] on 2025-03-26 17:34_

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

_Comment by @dhruvmanila on 2025-03-26 18:20_

> I don't think it was ever a _good_ idea to try `del __debug__`, so I think there's also an argument for not making this version-dependent at all. That would only simplify the implementation very slightly, though.

I'd be in favor of just making it version independent but happy to hear others thoughts on this.

---

_@dhruvmanila approved on 2025-03-26 18:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/options.rs`:23 on 2025-03-27 11:53_

I'd prefer to keep the type `Clone`. It makes it easier to add `non-copy` options in the future which doesn't seem entirely unlikely. 

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:329 on 2025-03-27 12:05_

I think there are more cases that need handling where we don't use an `ExprName` in the AST

```py
from x import __debug__
import __debug__

match x:
	__debug__: ...

f(__debug__ = b)
```

Finding the Identifier nodes is somewhat annoying because our visitor doesn't traverse `Identifier` nodes. You can take a look at where I believe I identified all `Identifier` nodes https://github.com/astral-sh/ruff/blob/e42a8daf6433884469360fd390472246fd4c0ca8/crates/red_knot_server/src/semantic/goto.rs#L68




---

_@MichaReiser reviewed on 2025-03-27 12:06_

---

_@ntBre reviewed on 2025-03-27 20:01_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:329 on 2025-03-27 20:01_

Ah good catch. I think I should have covered all of these now.

For `match` patterns, I moved the check into the `MultipleCaseAssignmentVisitor`, so it might be worth renaming that to something more generic like `MatchPatternVisitor` or just `PatternVisitor` now too.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:162 on 2025-03-27 21:00_

should we add a `check_identifier` function that handles the `if name == "__debug__"` handling to avoid some repetition?

---

_@MichaReiser approved on 2025-03-27 21:01_

---

_@MichaReiser approved on 2025-03-28 15:50_

---

_Merged by @ntBre on 2025-03-29 16:07_

---

_Closed by @ntBre on 2025-03-29 16:07_

---

_Branch deleted on 2025-03-29 16:07_

---
