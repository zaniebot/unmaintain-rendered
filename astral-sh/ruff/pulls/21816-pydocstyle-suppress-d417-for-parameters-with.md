```yaml
number: 21816
title: "[`pydocstyle`] Suppress `D417` for parameters with `Unpack` annotations"
type: pull_request
state: merged
author: phongddo
labels:
  - rule
  - docstring
assignees: []
merged: true
base: main
head: phongddo/d417-unpack-args
created_at: 2025-12-05T15:56:34Z
updated_at: 2025-12-08T19:00:06Z
url: https://github.com/astral-sh/ruff/pull/21816
synced_at: 2026-01-12T15:57:34Z
```

# [`pydocstyle`] Suppress `D417` for parameters with `Unpack` annotations

---

_@phongddo_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ruff/issues/8774

This PR fixes `pydocstyle` incorrectly flagging missing argument for arguments with `Unpack` type annotation by extracting the `kwarg` `D417` suppression logic into a helper function for future rules as needed.

## Problem Statement

The below example was incorrectly triggering `D417` error for missing `**kwargs` doc.

```python
class User(TypedDict):
    id: int
    name: str

def do_something(some_arg: str, **kwargs: Unpack[User]):
    """Some doc
    
    Args:
        some_arg: Some argument
    """
```

<img width="1135" height="276" alt="image" src="https://github.com/user-attachments/assets/42fa4bb9-61a5-4a70-a79c-0c8922a3ee66" />

`**kwargs: Unpack[User]` indicates the function expects keyword arguments that will be unpacked. Ideally, the individual fields of the User `TypedDict` should be documented, not in the `**kwargs` itself. The `**kwargs` parameter acts as a semantic grouping rather than a parameter requiring documentation.

## Solution

As discussed in the linked issue, it makes sense to suppress the `D417` for parameters with `Unpack` annotation. I extract a helper function to solely check `D417` should be suppressed with `**kwarg: Unpack[T]` parameter, this function can also be unit tested independently and reduce complexity of current `missing_args` check function. This also makes it easier to add additional rules in the future.

_✏️ Note:_ This is my first PR in this repo, as I've learned a ton from it, please call out anything that could be improved. Thanks for making this excellent tool 👏

## Test Plan

Add 2 test cases in `D417.py` and update snapshots.


---

_Marked ready for review by @phongddo on 2025-12-05 16:23_

---

_Label `rule` added by @ntBre on 2025-12-05 21:24_

---

_Label `docstring` added by @ntBre on 2025-12-05 21:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1848 on 2025-12-05 21:28_

Small nit here:

```suggestion
    parameter.annotation.is_some_and(|annotation|
        checker
            .semantic()
            .match_typing_expr(map_subscript(annotation), "Unpack")
    ) 
```

We sometimes make functions like this only take the `&SemanticModel` from `checker.semantic()` instead of a whole `&Checker` too, but I don't feel too strongly about that.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1841 on 2025-12-05 21:34_

Might be worth a short docstring here too, maybe something like:

```suggestion
/// Returns `true` if `parameter` is annotated with `typing.Unpack`
fn should_ignore_kwarg_check(checker: &Checker, parameter: &Parameter) -> bool {
```

Along those lines, a slightly better function name might be something like `has_unpack_annotation`.

---

_Comment by @astral-sh-bot[bot] on 2025-12-05 21:37_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@ntBre requested changes on 2025-12-05 21:50_

Thank you! This looks great to me overall, I just had a few very small nits. I think it may also be worth calling out this exception in the rule documentation, and possibly linking to the Python [docs](https://typing.python.org/en/latest/spec/callables.html#unpack-kwargs).

---

_@phongddo reviewed on 2025-12-05 21:59_

---

_Review comment by @phongddo on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1841 on 2025-12-05 21:59_

> Might be worth a short docstring

Agree! I can add docstring 👍 

> a slightly better function name might be something like `has_unpack_annotation`

I intentionally gave the function a more generic name in case we want to add additional rules for ignoring `kwarg` checks in the future, not just `Unpack[T]`. However, I'm fine making the change now, and we can revisit it later if needed. What do you think?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1841 on 2025-12-05 22:06_

Yeah I think I'd lean toward naming it for the way we use it now, unless we had future plans in mind. We can always rename it later, or just add additional, more specific helpers.

---

_@ntBre reviewed on 2025-12-05 22:06_

---

_@phongddo reviewed on 2025-12-05 22:10_

---

_Review comment by @phongddo on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1841 on 2025-12-05 22:10_

Sure, that sounds great 👍 

---

_Comment by @phongddo on 2025-12-05 22:28_

Hey @ntBre 👋 Thanks for reviewing the PR! I've addressed your comments and updated the rule documentation as you suggested. It's my first time writing a rule doc, so I'm not sure if it follows standard, would love your feedback 🙏

---

_Review requested from @ntBre by @phongddo on 2025-12-05 22:28_

---

_@phongddo reviewed on 2025-12-05 22:28_

---

_Review comment by @phongddo on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1841 on 2025-12-05 22:28_

Addressed in https://github.com/astral-sh/ruff/pull/21816/commits/1491d7674cd479cc986bfe363d337af3f4336ca0

---

_@phongddo reviewed on 2025-12-05 22:28_

---

_Review comment by @phongddo on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1848 on 2025-12-05 22:28_

Addressed in https://github.com/astral-sh/ruff/pull/21816/commits/1491d7674cd479cc986bfe363d337af3f4336ca0

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:1190 on 2025-12-08 18:51_

```suggestion
/// This follows the Python typing specification for unpacking keyword arguments.
```

---

_@ntBre approved on 2025-12-08 18:52_

Looks great, thank you!

---

_Renamed from "[`pydocstyle`] Suppress `D417` for parameters with `Unpack` annotation" to "[`pydocstyle`] Suppress `D417` for parameters with `Unpack` annotations" by @ntBre on 2025-12-08 18:52_

---

_Merged by @ntBre on 2025-12-08 19:00_

---

_Closed by @ntBre on 2025-12-08 19:00_

---
