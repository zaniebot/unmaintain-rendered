```yaml
number: 19023
title: "[`flake8-pyi`] Fix `PYI041` not resolving string annotations"
type: pull_request
state: open
author: robsdedude
labels:
  - rule
assignees: []
base: main
head: fix/pyi041-in-string-annotation
created_at: 2025-06-29T10:37:53Z
updated_at: 2025-10-11T08:55:04Z
url: https://github.com/astral-sh/ruff/pull/19023
synced_at: 2026-01-12T15:56:29Z
```

# [`flake8-pyi`] Fix `PYI041` not resolving string annotations

---

_@robsdedude_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

While working on https://github.com/astral-sh/ruff/pull/18637, I [noticed](https://github.com/astral-sh/ruff/pull/18637#discussion_r2142594045) that PYI041 does not properly resolve string annotations. This PR fixes that.

## Test Plan
A whole new file with tests has been added.


---

_Comment by @github-actions[bot] on 2025-06-29 10:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @robsdedude on 2025-06-29 13:37_

---

_Review requested from @AlexWaygood by @robsdedude on 2025-06-29 13:37_

---

_Review requested from @ntBre by @MichaReiser on 2025-07-07 12:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041_3.py`:16 on 2025-07-07 12:54_

Can you add a test for a type annotation consisting of a concatenated string literal:
```suggestion
def good1(arg: "int") -> "int" "| bool":
    ...
```

---

_@MichaReiser reviewed on 2025-07-07 12:55_

---

_@robsdedude reviewed on 2025-07-07 13:31_

---

_Review comment by @robsdedude on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041_3.py`:16 on 2025-07-07 13:31_

Good catch! While detection works, the fix did indeed drop the quotation marks.

---

_@robsdedude reviewed on 2025-07-07 15:14_

---

_Review comment by @robsdedude on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041_3.py`:16 on 2025-07-07 15:14_

This is non-trivial to fix. I'll have to go on a side-mission fixing https://github.com/astral-sh/ruff/issues/19184 first. 

My approach was to alter `parse_simple_type_annotation` (just like `parse_complex_type_annotation` already does) such that the resulting resolved virtual expression spans that whole string. That way, the rule can just replace the whole string and wrap it in quotes if the original annotation was a forward reference.

But alas, that change to `parse_simple_type_annotation` amplifies the linked bug to also affect non-concatenated forward refs.

---

_@MichaReiser reviewed on 2025-07-07 16:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041_3.py`:16 on 2025-07-07 16:02_

I believe this was part of the reason why we decided not to support string annotations in the initial implementation as they aren't as common anymore and we didn't felt like they're worth all the complexity they add. 

Could we disable the fix for problematic stringified annotations (I think you can also use triple quoted strings or put them across multiple lines). We should try to add tests for all of those if we decide that supporting them is worth the complexity

---

_@robsdedude reviewed on 2025-07-07 16:06_

---

_Review comment by @robsdedude on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041_3.py`:16 on 2025-07-07 16:06_

Tripple quoted type annotations actually work fine. There are some in the test set. 

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:552 on 2025-08-07 13:42_

Do we need to call `.ok()` here or can we just do

```suggestion
            let Ok(parsed_annotation) = self.parse_type_annotation(string_annotation) else {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:552 on 2025-08-07 14:38_

Hmm, maybe it makes sense to use `if-let` instead since both of the `else`s are the same:

```rust
if let ast::Expr::StringLiteral(string_annotation) = expr {
    if let Ok(parsed_annotation) = self.parse_type_annotation(string_annotation) {
        return map_fn(parsed_annotation.expression());
    }
}

map_fn(expr)
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:85 on 2025-08-07 14:43_

Is it possible just to return an `Expr` from the `map_maybe_stringized_annotation`? I think it would simplify things a bit if so. We could avoid passing a closure and just do something like:

```rust
check_annotation(checker, map_string_annotation(annotation), annotation)
```

That seems closer to another API I've seen in `map_callable`:

https://github.com/astral-sh/ruff/blob/96ffd8a134d62f5426c9d94d15adb21100d90f01/crates/ruff_python_ast/src/helpers.rs#L724-L727

Maybe it doesn't work here, though.

---

_@ntBre reviewed on 2025-08-07 14:55_

Thanks and sorry for the delay. This looks good to me, but I think we may need to gate this behind preview since it's a pretty big change to a stable rule.

Otherwise I just had some small ideas/suggestions, assuming @MichaReiser's concerns are resolved.

---

_Renamed from "[`flake8_pyi`] Fix `PYI041` not resolving string annotations" to "[`flake8-pyi`] Fix `PYI041` not resolving string annotations" by @ntBre on 2025-08-07 14:55_

---

_Label `rule` added by @ntBre on 2025-08-07 14:55_

---

_@robsdedude reviewed on 2025-10-11 08:18_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_numeric_union.rs`:85 on 2025-10-11 08:18_

Ah yes, it just requires a fair bit of lifetime juggling :D

---

_@robsdedude reviewed on 2025-10-11 08:39_

---

_Review comment by @robsdedude on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI041_3.py`:16 on 2025-10-11 08:39_

I ended up addressing this in a similar way as RUF013 handles is:
 * analysis on stringized annotations: will do
 * suggesting fixes on stringized annotations: only if simple sting (i.e., non-concatendated) and always mark as unsafe.

---

_Review requested from @ntBre by @robsdedude on 2025-10-11 08:40_

---

_Comment by @robsdedude on 2025-10-11 08:47_

@ntBre @MichaReiser Could you have another look. I think I addressed all comments.

Note to myself: when this is merged, take another look at https://github.com/astral-sh/ruff/issues/19184 to see if the same approach (https://github.com/astral-sh/ruff/pull/19023#discussion_r2422617499) could be applied.

---
