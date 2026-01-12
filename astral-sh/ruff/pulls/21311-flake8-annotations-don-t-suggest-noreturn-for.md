```yaml
number: 21311
title: "[`flake8-annotations`] Don't suggest `NoReturn` for functions raising `NotImplementedError` (`ANN201`, `ANN202`, `ANN205`, `ANN206`)"
type: pull_request
state: open
author: danparizher
labels:
  - fixes
assignees: []
base: main
head: fix-18886
created_at: 2025-11-07T05:08:03Z
updated_at: 2025-12-01T19:26:21Z
url: https://github.com/astral-sh/ruff/pull/21311
synced_at: 2026-01-12T15:57:20Z
```

# [`flake8-annotations`] Don't suggest `NoReturn` for functions raising `NotImplementedError` (`ANN201`, `ANN202`, `ANN205`, `ANN206`)

---

_@danparizher_

## Summary

Fix ANN201, ANN202, ANN205, and ANN206 to not automatically suggest `NoReturn`/`Never` return type annotations for functions that raise `NotImplementedError`. These rules will still flag missing return type annotations but won't provide an auto-fix, allowing developers to manually specify the actual return type that implementations should return.

Fixes #18886

## Problem Analysis

The bug occurred because the `auto_return_type` function in `flake8_annotations/helpers.rs` detects when all control flow paths in a function raise an exception (`Terminal::Raise`) and automatically suggests `NoReturn`/`Never` as the return type. However, `NotImplementedError` is semantically different from other exceptions - it's used to indicate abstract methods that should be implemented by subclasses, not methods that truly never return.

When a function raises `NotImplementedError`, it's typically an abstract method pattern where:
- The base class defines the method signature but doesn't implement it
- Subclasses are expected to override the method with a proper implementation
- The return type should reflect what implementations will actually return, not `NoReturn`

For example, in Django's serializer pattern:

```py
class BaseSerializer:
    def to_internal_value(self, data):
        raise NotImplementedError('`to_internal_value()` must be implemented.')
```

This method should have a return type annotation like `dict` or `Any`, not `NoReturn`, because implementations will return actual values.

## Approach

The fix modifies the `auto_return_type` function to:

1. **Added semantic analysis capability**: Modified `auto_return_type` to accept a `SemanticModel` parameter, enabling it to analyze exception types in raise statements.

2. **Created detection helper**: Implemented `only_raises_not_implemented_error()` function that:
   - Traverses the function body using a custom visitor to collect all `raise` statements
   - Checks if all raises are `NotImplementedError` (or `NotImplemented`) using semantic analysis
   - Returns `true` only if all raises are `NotImplementedError`

3. **Modified return type inference**: When `Terminal::Raise` is detected, the function now:
   - First checks if all raises are `NotImplementedError`
   - If yes, returns `None` (no auto-fix suggested, but diagnostic still emitted)
   - If no, returns `AutoPythonType::Never` as before (suggesting `NoReturn`/`Never`)

4. **Updated all call sites**: Updated all 4 call sites in `definition.rs` to pass `checker.semantic()`:
   - ANN201 (public functions)
   - ANN202 (private functions)  
   - ANN205 (static methods)
   - ANN206 (class methods)

The fix ensures that:
- Functions raising `NotImplementedError` still get flagged for missing return type annotations
- But no auto-fix is suggested, allowing manual specification of the correct return type
- Functions raising other exceptions (like `ValueError`) still correctly get `NoReturn`/`Never` suggestions

---

_Comment by @github-actions[bot] on 2025-11-07 05:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-11-07 20:56_

Could you summarize the similarities/differences from #18927? I quickly skimmed both implementations and this looks similar to the earlier PR. Has @MichaReiser's concern in https://github.com/astral-sh/ruff/pull/18927#discussion_r2166250598 been addressed?

---

_Label `fixes` added by @ntBre on 2025-11-07 20:57_

---

_Comment by @danparizher on 2025-11-07 23:00_

I missed that, thanks for bringing it back up. I just refactored the implementation:

- Adds a new `Terminal::RaiseNotImplemented` variant that's detected during the initial `Terminal::from_function()` traversal
- Eliminates the need for the `only_raises_not_implemented_error()` function that was doing a second pass
- Updates `auto_return_type()` to check for `Terminal::RaiseNotImplemented` directly

The way I did it in the original PR is pretty similar, minus the logic that I just implemented

---

_Review requested from @ntBre by @amyreese on 2025-11-26 01:51_

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/terminal.rs`:164 on 2025-12-01 19:16_

I looked at the references to `from_body`, and I don't think there are any cases where a semantic model is unavailable. It's only passed as `None` in `sometimes_breaks`, but `sometimes_breaks` is only called from `from_body`, which has access to a semantic model, unless  I'm missing something.

I also think we could use `has_builtin_binding` here.

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/terminal.rs`:15 on 2025-12-01 19:19_

Did you check if all of the other uses of `Terminal::Raise` should now be something like this?

```rust
matches!(terminal, Terminal::Raise | Terminal::RaiseNotImplemented)
```

It doesn't look like any other tests or ecosystem results are changing, but at least from a quick look, I would think these variants should be treated the same in some other rules. If so, it might be good to add a `Terminal::is_raise` helper method with a `matches!` like the one above.

---

_Review comment by @ntBre on `crates/ruff_python_semantic/src/analyze/terminal.rs`:203 on 2025-12-01 19:20_

Should we combine this with the arms above?


```suggestion
            // If one of the operators is `RaiseNotImplemented`, treat it like `Raise` for combinations
            (Self::Raise | Self::RaiseNotImplemented, Self::ConditionalReturn) => Self::RaiseOrReturn,
            (Self::ConditionalReturn, Self::Raise | Self::RaiseNotImplemented) => Self::RaiseOrReturn,
```

or maybe just group the lines and not separate them with a comment.

Hmm, after scrolling through a few more of these, it might be easier if every `Self::Raise` just became `Self::Raise | Self::RaiseNotImplemented` except where the behavior should differ. Otherwise we have to manually compare the two everywhere they appear.

---

_@ntBre requested changes on 2025-12-01 19:26_

Thanks, this looks reasonable to me overall. I just think we can make the `SemanticModel` a  required argument, and we should double check all of the existing uses of `Terminal::Raise` to be sure they handle `Terminal::RaiseNotImplemented` correctly.

---
