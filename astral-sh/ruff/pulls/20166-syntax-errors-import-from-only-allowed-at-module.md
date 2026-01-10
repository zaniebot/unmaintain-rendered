```yaml
number: 20166
title: "[syntax-errors]: import from * only allowed at module scope (F406)"
type: pull_request
state: merged
author: 11happy
labels:
  - internal
assignees: []
merged: true
base: main
head: F406
created_at: 2025-08-30T12:23:23Z
updated_at: 2025-09-16T19:53:28Z
url: https://github.com/astral-sh/ruff/pull/20166
synced_at: 2026-01-10T17:40:28Z
```

# [syntax-errors]: import from * only allowed at module scope (F406)

---

_Pull request opened by @11happy on 2025-08-30 12:23_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR implements F406 https://docs.astral.sh/ruff/rules/undefined-local-with-nested-import-star-usage/ as a semantic syntax error

## Test Plan

I have written inline tests as directed in #17412 


---

_Review requested from @MichaReiser by @11happy on 2025-08-30 12:23_

---

_Review requested from @dhruvmanila by @11happy on 2025-08-30 12:23_

---

_Renamed from "F406" to "[syntax-errors]: import from * only allowed at module scope" by @11happy on 2025-08-30 12:27_

---

_Renamed from "[syntax-errors]: import from * only allowed at module scope" to "[syntax-errors]: import from * only allowed at module scope (F406)" by @11happy on 2025-08-30 12:27_

---

_Review requested from @carljm by @11happy on 2025-09-01 15:26_

---

_Review requested from @AlexWaygood by @11happy on 2025-09-01 15:26_

---

_Review requested from @sharkdp by @11happy on 2025-09-01 15:26_

---

_Review requested from @dcreager by @11happy on 2025-09-01 15:26_

---

_Comment by @11happy on 2025-09-01 15:27_

I have fixed the md file test which was causing CI to fail.

Thank you

---

_Comment by @github-actions[bot] on 2025-09-02 13:16_

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

_Review request for @carljm removed by @carljm on 2025-09-02 17:46_

---

_Review requested from @ntBre by @dhruvmanila on 2025-09-03 04:31_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/semantic_errors.rs`:73 on 2025-09-03 04:34_

Let's add a few more test cases like including it in class scope, module scope, etc. to check both the error and non-error case. The non-error case would need to go under `test_ok` (instead of `test_err`).

---

_@dhruvmanila reviewed on 2025-09-03 04:35_

---

_@11happy reviewed on 2025-09-04 16:08_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:73 on 2025-09-04 16:08_

done

---

_Comment by @11happy on 2025-09-04 16:12_

have resolved the merge conflict as well.
Thank you

---

_Comment by @11happy on 2025-09-07 16:13_

Hii @ntBre, would appreciate your feedback on this when you get a chance. 
Thank you : )

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:663 on 2025-09-12 17:14_

I know this is the name of the lint rule, but I think it would make sense to rename the syntax error to something like `LocalImportStar` or `NonModuleImportStar`. For the purpose of the syntax error, we don't actually care about the `undefined-local` part, it's still a syntax error even with no local variables:

```pycon
>>> def foo():
...     from math import *
...
  File "<python-input-7>", line 2
    from math import *
                     ^
SyntaxError: import * only allowed at module level
```

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:73 on 2025-09-12 17:18_

I see you added a module-scoped test, would you mind throwing in a class scope like Dhruv also suggested? It can be part of the same `test_err` block as the function:

```rust
// test_err import_from_star
// def f():
//     from module import *
// class C:
//     from module import *
```

I think that will cover all of the scopes where you can actually have an import statement.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:80 on 2025-09-12 17:26_

I guess we don't have any tests covering this, but this looks very different from the code used in the lint rule:

https://github.com/astral-sh/ruff/blob/f507ff8cd036fd2d7ab8ad13f1a207555bc7596d/crates/ruff_linter/src/checkers/ast/analyze/statement.rs#L815-L820

Should we be using `format_import_from` again here?

For an example test, the `level` argument for this would be 3:

```py
from ...math import *
```

I think without the helper function we would format that as just `math`.

---

_@ntBre reviewed on 2025-09-12 17:34_

Thanks, this looks good overall, just a few smaller suggestions. While we're here, can we also update this comment:

https://github.com/astral-sh/ruff/blob/f507ff8cd036fd2d7ab8ad13f1a207555bc7596d/crates/ruff_linter/src/checkers/ast/analyze/unresolved_references.rs#L16-L22

I was getting confused about this other case of F406 we're not handling, but I finally realized that the comment is just wrong. It should say `F405`, which is not a syntax error.

---

_@11happy reviewed on 2025-09-13 13:34_

---

_Review comment by @11happy on `crates/ruff_linter/src/checkers/ast/mod.rs`:663 on 2025-09-13 13:34_

done

---

_@11happy reviewed on 2025-09-13 13:35_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:80 on 2025-09-13 13:35_

yes , I checked without helper function it was just math, I have used helper function likewise.

---

_@11happy reviewed on 2025-09-13 13:38_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:73 on 2025-09-13 13:38_

done

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:79 on 2025-09-15 22:59_

Would you mind adding one more test? `from module import *, *` is already a separate syntax error, but I'm wondering how many `NonModuleImportStar` errors we should emit in this case. We don't emit any `F406`s because the `Star import must be the only import` error is a parse error, but the interaction between the two syntax errors will be interesting in the future when we make `F406` a syntax error.

With the `break` on line 90, I think we'll only emit one `NonModuleImportStar` and one parse error, which seems reasonable to me.

---

_@ntBre reviewed on 2025-09-15 23:00_

Thank you! This looks great to me. I just had one more test idea for a future edge case.

---

_Review request for @MichaReiser removed by @ntBre on 2025-09-15 23:00_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-09-15 23:00_

---

_Review request for @sharkdp removed by @ntBre on 2025-09-15 23:00_

---

_Review request for @dcreager removed by @ntBre on 2025-09-15 23:00_

---

_@11happy reviewed on 2025-09-15 23:17_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:79 on 2025-09-15 23:17_

done

---

_@ntBre approved on 2025-09-16 19:49_

Thank you!

---

_Label `internal` added by @ntBre on 2025-09-16 19:52_

---

_Merged by @ntBre on 2025-09-16 19:53_

---

_Closed by @ntBre on 2025-09-16 19:53_

---
