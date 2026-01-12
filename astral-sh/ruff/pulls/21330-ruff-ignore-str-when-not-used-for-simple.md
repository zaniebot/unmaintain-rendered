```yaml
number: 21330
title: "[`ruff`] Ignore `str()` when not used for simple conversion (`RUF065`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-21315
created_at: 2025-11-07T23:27:05Z
updated_at: 2025-11-10T23:18:08Z
url: https://github.com/astral-sh/ruff/pull/21330
synced_at: 2026-01-12T15:57:21Z
```

# [`ruff`] Ignore `str()` when not used for simple conversion (`RUF065`)

---

_@danparizher_

## Summary

Fixed RUF065 (`logging-eager-conversion`) to only flag `str()` calls when they perform a simple conversion that can be safely removed. The rule now ignores `str()` calls with no arguments, multiple arguments, starred arguments, or keyword unpacking, preventing false positives.

Fixes #21315

## Problem Analysis

The RUF065 rule was incorrectly flagging all `str()` calls in logging statements, even when `str()` was performing actual conversion work beyond simple type coercion. Specifically, the rule flagged:

- `str()` with no arguments - which returns an empty string
- `str(b"data", "utf-8")` with multiple arguments - which performs encoding conversion
- `str(*args)` with starred arguments - which unpacks arguments
- `str(**kwargs)` with keyword unpacking - which passes keyword arguments

These cases cannot be safely removed because `str()` is doing meaningful work (encoding conversion, argument unpacking, etc.), not just redundant type conversion.

The root cause was that the rule only checked if the function was `str()` without validating the call signature. It didn't distinguish between simple `str(value)` conversions (which can be removed) and more complex `str()` calls that perform actual work.

## Approach

The fix adds validation to the `str()` detection logic in `logging_eager_conversion.rs`:

1. **Check argument count**: Only flag `str()` calls with exactly one positional argument (`str_call_args.args.len() == 1`)
2. **Check for starred arguments**: Ensure the single argument is not starred (`!str_call_args.args[0].is_starred_expr()`)
3. **Check for keyword arguments**: Ensure there are no keyword arguments (`str_call_args.keywords.is_empty()`)

This ensures the rule only flags cases like `str(value)` where `str()` is truly redundant and can be removed, while ignoring cases where `str()` performs actual conversion work.

The fix maintains backward compatibility - all existing valid test cases continue to be flagged correctly, while the new edge cases are properly ignored.

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:159 on 2025-11-08 02:42_

`logging.warning("%s", str(object="!"))`, with one keyword argument, should still be flagged.

---

_@dscorbett reviewed on 2025-11-08 02:42_

---

_Comment by @danparizher on 2025-11-08 04:43_

I added a test case for `str(object="!")`, and the new logic seems to work in isolation, but for some reason isn't being detected in the test file. I would appreciate it if someone could help me diagnose the issue!

---

_Label `bug` added by @ntBre on 2025-11-10 17:24_

---

_Label `rule` added by @ntBre on 2025-11-10 17:24_

---

_Label `preview` added by @ntBre on 2025-11-10 17:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:162 on 2025-11-10 17:31_

This looks pretty complicated. Can we just use `find_argument` and a check that `str_call_args.len() == 1`?


```suggestion
                    if checker.semantic().match_builtin_expr(func.as_ref(), "str")
                        && str_call_args.len() == 1
                        && str_call_args.find_argument("object", 0).is_some_and(|arg| !arg.is_variadic()) => 
```

with 

```rust
impl<'a> ArgOrKeyword<'a> {
    pub const fn is_variadic(self) -> bool {
        match self {
            ArgOrKeyword::Arg(expr) => expr.is_starred_expr(),
            ArgOrKeyword::Keyword(keyword) => keyword.arg.is_none(),
        }
    }
}
````

This comes up a lot, so that might be a nice helper to have around.

Hopefully this will help with the `object` case, but I'm not sure.

---

_@ntBre requested changes on 2025-11-10 17:38_

Thanks! I had a suggestion for simplification, which will hopefully help with the test case too.

---

_Comment by @danparizher on 2025-11-10 21:35_

Thanks! I pushed those changes, but it looks like we're running into the same issue as before. The fix works in isolation but not within the test.

---

_Comment by @astral-sh-bot[bot] on 2025-11-10 21:45_


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

_Comment by @ntBre on 2025-11-10 21:46_

Oh, `str` is shadowed on line 37 of the test file, so none of the new tests are actually exercising the new logic. I think we should add the new tests to a separate file.

---

_@ntBre approved on 2025-11-10 22:55_

Thank you!

---

_Merged by @ntBre on 2025-11-10 23:04_

---

_Closed by @ntBre on 2025-11-10 23:04_

---

_Branch deleted on 2025-11-10 23:18_

---
