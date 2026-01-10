```yaml
number: 16443
title: "Document `LinterResult::has_syntax_error` and add `Parsed::has_no_errors`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - parser
assignees: []
merged: true
base: main
head: brent/caching-followup
created_at: 2025-02-28T17:18:38Z
updated_at: 2025-03-04T13:35:40Z
url: https://github.com/astral-sh/ruff/pull/16443
synced_at: 2026-01-10T19:49:01Z
```

# Document `LinterResult::has_syntax_error` and add `Parsed::has_no_errors`

---

_Pull request opened by @ntBre on 2025-02-28 17:18_

Summary
--

This is a follow up addressing the comments on #16425. As @dhruvmanila pointed out, the naming is a bit tricky. I went with `has_no_errors` to try to differentiate it from `is_valid`. It actually ends up negated in most uses, so it would be more convenient to have `has_any_errors` or `has_errors`, but I thought it would sound too much like the opposite of `is_valid` in that case. I'm definitely open to suggestions here.

Test Plan
--

Existing tests.


---

_Label `internal` added by @ntBre on 2025-02-28 17:18_

---

_Label `parser` added by @ntBre on 2025-02-28 17:18_

---

_Review requested from @MichaReiser by @ntBre on 2025-02-28 17:18_

---

_Review requested from @dhruvmanila by @ntBre on 2025-02-28 17:18_

---

_Comment by @github-actions[bot] on 2025-02-28 17:27_

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

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:361 on 2025-03-03 05:54_

It seems that we use `!has_no_errors` in most places so I'd optimize for that instead i.e., use `has_errors` or `has_any_error` or `contains_errors` or `has_syntax_errors` (similar to the field on `LinterResult`

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/fixtures.rs`:42 on 2025-03-03 05:55_

I think we should now rename the variable `is_valid` to match the method name. If it's only being used once we can just inline the method call there instead.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/fixtures.rs`:104 on 2025-03-03 05:55_

Same as above.

---

_@dhruvmanila approved on 2025-03-03 05:57_

Thanks for doing this.

Regardless of the name choice, I think it might be useful to keep it consistent throughout the code base i.e., keep the method name same as variable name and field name on `LinterResult`

---

_@MichaReiser reviewed on 2025-03-03 07:58_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:351 on 2025-03-03 07:58_

Not specifically related to this PR but it seems a good opportunity. How about:

* Add a method `has_invalid_syntax` or `is_invalid_syntax` as the opposite of `is_valid` (we could also rename `is_valid` to `has_valid_syntax` or `is_valid_syntax`. It's still somewhat ambigious by what it mean but I think it works well with unsupported syntax (which technically is valid, it's just unsupported)
* Rename the new method to `has_syntax_errors` and `has_no_syntax_errors`?


---

_@MichaReiser approved on 2025-03-03 07:58_

---

_Review requested from @carljm by @ntBre on 2025-03-03 15:26_

---

_Review requested from @AlexWaygood by @ntBre on 2025-03-03 15:26_

---

_Review requested from @sharkdp by @ntBre on 2025-03-03 15:26_

---

_Comment by @ntBre on 2025-03-03 15:26_

Thanks for the suggestions! There are now four methods here:

* `has_valid_syntax`
* `has_invalid_syntax`
* `has_syntax_errors`
* `has_no_syntax_errors`

and I checked the references for each of the original methods and replaced negations with the new, negated methods.

I also inlined the `is_valid` variables that @dhruvmanila pointed out.

---

_@MichaReiser reviewed on 2025-03-03 15:54_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:354 on 2025-03-03 15:54_

I think i'd prefer to use the term parse errors or a more detailed description over using *syntax errors*. The description otherwise doesn't make it clear how it is different from `has_syntax_erros`. 

But it's tricky with all those being syntax errors. 

Another option is to rename `unsupported_syntax_errors` to `has_unsupported_syntax` (which would match `has_invalid_syntax`) or `has_no_unsupported_syntax`. 



---

_@ntBre reviewed on 2025-03-03 15:58_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/lib.rs`:354 on 2025-03-03 15:58_

Oh good catch. I can even link to the `ParseError` and `UnsupportedSyntaxError` types like I did in the new methods.

---

_Merged by @ntBre on 2025-03-04 13:35_

---

_Closed by @ntBre on 2025-03-04 13:35_

---

_Branch deleted on 2025-03-04 13:35_

---
