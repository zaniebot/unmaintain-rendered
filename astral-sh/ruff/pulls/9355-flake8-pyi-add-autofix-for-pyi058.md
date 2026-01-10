```yaml
number: 9355
title: "[flake8-pyi] Add autofix for PYI058"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: pyi058-autofix
created_at: 2024-01-02T12:01:14Z
updated_at: 2024-01-03T16:30:36Z
url: https://github.com/astral-sh/ruff/pull/9355
synced_at: 2026-01-10T23:07:18Z
```

# [flake8-pyi] Add autofix for PYI058

---

_Pull request opened by @AlexWaygood on 2024-01-02 12:01_

## Summary

This PR adds an autofix for the newly added PYI058 rule (added in #9313). ~~The PR's current implementation is that the fix is only available if the fully qualified name of `Generator` or `AsyncGenerator` is being used:~~
- ~~`-> typing.Generator` is converted to `-> typing.Iterator`;~~
- ~~`-> collections.abc.AsyncGenerator[str, Any]` is converted to `-> collections.abc.AsyncIterator[str]`;~~
- ~~but `-> Generator` is _not_ converted to `-> Iterator`. (It would require more work to figure out if `Iterator` was already imported or not. And if it wasn't, where should we import it from? `typing`, `typing_extensions`, or `collections.abc`? It seems much more complicated.)~~

The fix is marked as always safe for `__iter__` or `__aiter__` methods in `.pyi` files, but unsafe for all such methods in `.py` files that have more than one statement in the method body.

This felt slightly fiddly to accomplish, but I couldn't _see_ any utilities in https://github.com/astral-sh/ruff/tree/main/crates/ruff_linter/src/fix that would have made it simpler to implement. Lmk if I'm missing something, though -- my first time implementing an autofix! :)

## Test Plan

`cargo test` / `cargo insta review`.


---

_Comment by @github-actions[bot] on 2024-01-02 12:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-01-02 14:07_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:339 on 2024-01-02 14:07_

So, one thing you can explore here: take a look at `get_or_import_symbol` on `Importer`. It lets you define a module-and-member, and then gives you (1) the edit required to import the symbol (if not present), and (2) the bound name of the symbol. It should thus allow you to support cases in which the symbol is not accessed as a fully-qualified name.

---

_@Skylion007 reviewed on 2024-01-02 15:46_

---

_Review comment by @Skylion007 on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI058.py`:25 on 2024-01-02 15:46_

`typing.Generator[str, None, None]` is a really common usecase I see in pratice and we may want to add a test that it can probably change it to `typing.Iterator[str]`

---

_@AlexWaygood reviewed on 2024-01-03 14:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI058.py`:25 on 2024-01-03 14:53_

Added an explicit test for that 👍

---

_@AlexWaygood reviewed on 2024-01-03 14:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:339 on 2024-01-03 14:54_

Thanks, that was easier than I expected! The autofix now also fixes things even if it's not a fully qualified name.

---

_Review requested from @charliermarsh by @AlexWaygood on 2024-01-03 14:54_

---

_@charliermarsh reviewed on 2024-01-03 15:58_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:300 on 2024-01-03 15:58_

@AlexWaygood - I did some light refactoring here to use enums for all the different values rather than strings. You might be interested in taking a look. I think it helps enforce some of the invariants that are expected throughout the code and make it clear which values are valid in various places.

---

_@charliermarsh reviewed on 2024-01-03 15:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI058.py`:2 on 2024-01-03 15:59_

Unfortunately, I had to scope each test here into its own function so that they can reason about the imports independently.

---

_@charliermarsh reviewed on 2024-01-03 15:59_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:267 on 2024-01-03 15:59_

@AlexWaygood - I tried to generalize this so that it doesn't need different logic for attribute vs. name.

---

_@charliermarsh reviewed on 2024-01-03 16:00_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI058_PYI058.pyi.snap`:116 on 2024-01-03 16:00_

This is a bug in `get_or_import_symbol` (it doesn't know that it can reuse `collections.abc` because it's a two-part module). Need to fix.

---

_@AlexWaygood reviewed on 2024-01-03 16:00_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI058.py`:2 on 2024-01-03 16:00_

Ahh, that makes sense, was wondering if that would be an issue

---

_@charliermarsh approved on 2024-01-03 16:06_

---

_@charliermarsh reviewed on 2024-01-03 16:06_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI058_PYI058.pyi.snap`:116 on 2024-01-03 16:06_

I'll fix this separately, since the current fix isn't "wrong", it's just suboptimal.

---

_@charliermarsh reviewed on 2024-01-03 16:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:285 on 2024-01-03 16:10_

@AlexWaygood - A bit advanced but I changed this to use a lifetime so that it doesn't need to clone the expression. We know the expression will "live" long enough, so it's okay for us to use a reference.

---

_@charliermarsh reviewed on 2024-01-03 16:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:83 on 2024-01-03 16:10_

@AlexWaygood - I removed this since it's enforced in `let (method, module, member) = {` below.

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:64 on 2024-01-03 16:11_

@AlexWaygood - I changed this to use the enums, which implement `std::fmt::Display`, so we can still inline them in the `format!` call and automatically get the expected formatting.

---

_@charliermarsh reviewed on 2024-01-03 16:11_

---

_@charliermarsh reviewed on 2024-01-03 16:11_

Excellent work!

---

_Merged by @charliermarsh on 2024-01-03 16:11_

---

_Closed by @charliermarsh on 2024-01-03 16:11_

---

_Label `autofix` added by @charliermarsh on 2024-01-03 16:11_

---

_Label `rule` added by @charliermarsh on 2024-01-03 16:11_

---

_Branch deleted on 2024-01-03 16:18_

---

_@AlexWaygood reviewed on 2024-01-03 16:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:285 on 2024-01-03 16:20_

Nice, thanks! I've read about things being generic over lifetimes but have yet to successfully use the feature :)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:293 on 2024-01-03 16:21_

For my education: what's the advantage of implementing the trait here rather than just reading the attribute?

---

_@AlexWaygood reviewed on 2024-01-03 16:21_

---

_@AlexWaygood reviewed on 2024-01-03 16:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:300 on 2024-01-03 16:22_

Yeah, this is nice, thanks! Fewer magic strings everywhere

---

_@charliermarsh reviewed on 2024-01-03 16:27_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:293 on 2024-01-03 16:27_

It's really minor, more of a consistency thing... If you implement `Ranged`, then you get methods like `.start()` and `.end()` instead of having to do `.range.start()`.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/bad_generator_return_type.rs`:267 on 2024-01-03 16:30_

Nice, I didn't realise how powerful `get_or_import_symbol` was

---

_@AlexWaygood reviewed on 2024-01-03 16:30_

---

_Comment by @AlexWaygood on 2024-01-03 16:30_

Thanks!

---
