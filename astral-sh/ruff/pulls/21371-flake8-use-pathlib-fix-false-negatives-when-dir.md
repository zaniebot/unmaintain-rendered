```yaml
number: 21371
title: "[`flake8-use-pathlib`] Fix false negatives when `dir_fd=None` is explicitly passed (`PTH106`, `PTH107`, `PTH108`, `PTH115`)"
type: pull_request
state: open
author: danparizher
labels:
  - rule
assignees: []
base: main
head: fix-21342
created_at: 2025-11-11T00:19:16Z
updated_at: 2025-12-08T23:37:03Z
url: https://github.com/astral-sh/ruff/pull/21371
synced_at: 2026-01-12T15:57:22Z
```

# [`flake8-use-pathlib`] Fix false negatives when `dir_fd=None` is explicitly passed (`PTH106`, `PTH107`, `PTH108`, `PTH115`)

---

_@danparizher_

## Summary

Fixes false negatives in PTH106 (`os-rmdir`), PTH107 (`os-remove`), PTH108 (`os-unlink`), and PTH115 (`os-readlink`) when `dir_fd=None` is explicitly passed. These rules now correctly trigger violations even when the default `dir_fd` value is explicitly set.

Fixes #21342

## Problem Analysis

The `check_os_pathlib_single_arg_calls` helper function checked `call.arguments.len() != 1` to ensure exactly one argument. However, `Arguments.len()` counts both positional and keyword arguments together.

When `dir_fd=None` was explicitly passed (e.g., `os.rmdir("path", dir_fd=None)`), the call had 2 arguments total (1 positional + 1 keyword), causing the function to return early and preventing the rule from triggering.

## Approach

Modified `check_os_pathlib_single_arg_calls` to:

1. Check for exactly one positional argument OR the main argument passed as a keyword
2. Allow `dir_fd=None` as an additional keyword argument when the main argument is present
3. Continue to suppress rules when `dir_fd` has a non-default value (since pathlib doesn't support it)

This ensures consistent behavior across all single-argument pathlib replacement rules (PTH106, PTH107, PTH108, PTH115, PTH202, PTH203).

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 00:30_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Label `rule` added by @ntBre on 2025-11-11 13:39_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:62 on 2025-11-11 13:50_

Is it possible that we just needed to delete this check? I see that `is_keyword_only_argument_non_default` is already being called in at least PTH115:

https://github.com/astral-sh/ruff/blob/7b237d316f4099c75811527cb13dd8699ec23d1c/crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_readlink.rs#L71-L79

So I don't think we should call it again here in the helper, but simply deleting the check may also not be enough.

It might be worth checking one of the other PTH rules that already accepts a `dir_fd` argument to see if we can copy how they work.

---

_@ntBre requested changes on 2025-11-11 13:52_

---

_@danparizher reviewed on 2025-11-11 21:43_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:62 on 2025-11-11 21:43_

That makes sense to me. I removed the check from the helper (it was already being done by all four callers: `PTH106`, `PTH107`, `PTH108`, `PTH115`).

---

_Review requested from @ntBre by @danparizher on 2025-11-11 22:20_

---

_@ntBre reviewed on 2025-11-18 20:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:84 on 2025-11-18 20:36_

Do we need these checks? I think we already validated all of the arguments. I tried deleting this, and all of the tests still pass.

---

_Review requested from @ntBre by @danparizher on 2025-11-20 00:02_

---

_@ntBre reviewed on 2025-12-08 23:37_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_use_pathlib/helpers.rs`:84 on 2025-12-08 23:37_

I think I intended for my comment to apply to this whole range, not just the last couple of checks. However, I realize now that we do need some kind of argument validation for these rules, but the current test cases don't really cover it. Locally I tried removing all of this validation code, and all of the tests still pass. As one example, the rule, or at least the fix, should not apply in this case:

```py
os.rmdir(p, unknown_keyword=None)
```

but this triggers the diagnostic and fix on this branch.

I'm kind of inclined not to call `check_os_pathlib_single_arg_calls` in these rules and instead to do all of the validation in the rules themselves. That seems closer to how we handle PTH101, for example:

https://github.com/astral-sh/ruff/blob/df296fbac1795054cb3b14c4f728c1360298ed92/crates/ruff_linter/src/rules/flake8_use_pathlib/rules/os_chmod.rs#L101-L106

The nice thing about this is that we can also emit a diagnostic in the presence of unknown arguments but suppress the fix.

In summary, I think we should:
1. Revert the changes to `check_os_pathlib_single_arg_calls` because I think enforcing exactly one argument is actually correct for some of the rules that use it
2. Inline `check_os_pathlib_single_arg_calls` into the rules in this PR (`PTH106`, `PTH107`, `PTH108`, and `PTH115`)
3. Modify these rules to validate their own arguments and emit a diagnostic without a fix in the presence of unknown arguments
4. Add more tests exercising the argument validation logic

---
