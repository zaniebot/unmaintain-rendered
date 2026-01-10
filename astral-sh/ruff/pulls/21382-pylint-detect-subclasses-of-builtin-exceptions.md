```yaml
number: 21382
title: "[`pylint`] Detect subclasses of builtin exceptions (`PLW0133`)"
type: pull_request
state: merged
author: LoicRiegel
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/detect-custom-exception-in-useless_expression_statement
created_at: 2025-11-11T15:23:32Z
updated_at: 2025-12-10T07:42:18Z
url: https://github.com/astral-sh/ruff/pull/21382
synced_at: 2026-01-10T16:42:11Z
```

# [`pylint`] Detect subclasses of builtin exceptions (`PLW0133`)

---

_Pull request opened by @LoicRiegel on 2025-11-11 15:23_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Closes #17347

Goal is to detect the useless exception statement not just for builtin exceptions but also custom (user defined) ones.

## Test Plan

<!-- How was it tested? -->
I added test cases in the rule fixture and updated the insta snapshot.
Note that I first moved up a test case case which was at the bottom to the correct "violation category".
I wasn't sure if I should create new test cases or just insert inside those tests. I know that ideally each test case should test only one thing, but here, duplicating twice 12 test cases seemed very verbose, and actually less maintainable in the future. The drawback is that the diff in the snapshot is hard to review, sorry. But you can see that the snapshot gives 38 diagnostics, which is what we expect.

Alternatively, I also created this file for manual testing.
```py
# tmp/test_error.py

class MyException(Exception):
    ...
class MyBaseException(BaseException):
    ...
class MyValueError(ValueError):
    ...
class MyExceptionCustom(Exception):
    ...
class MyBaseExceptionCustom(BaseException):
    ...
class MyValueErrorCustom(ValueError):
    ...
class MyDeprecationWarning(DeprecationWarning):
    ...
class MyDeprecationWarningCustom(MyDeprecationWarning):
    ...
class MyExceptionGroup(ExceptionGroup):
    ...
class MyExceptionGroupCustom(MyExceptionGroup):
    ...
class MyBaseExceptionGroup(ExceptionGroup):
    ...
class MyBaseExceptionGroupCustom(MyBaseExceptionGroup):
    ...


def foo():
    Exception("...")
    BaseException("...")
    ValueError("...")
    RuntimeError("...")
    DeprecationWarning("...")
    GeneratorExit("...")
    SystemExit("...")
    ExceptionGroup("eg", [ValueError(1), TypeError(2), OSError(3), OSError(4)])
    BaseExceptionGroup("eg", [ValueError(1), TypeError(2), OSError(3), OSError(4)])
    MyException("...")
    MyBaseException("...")
    MyValueError("...")
    MyExceptionCustom("...")
    MyBaseExceptionCustom("...")
    MyValueErrorCustom("...")
    MyDeprecationWarning("...")
    MyDeprecationWarningCustom("...")
    MyExceptionGroup("...")
    MyExceptionGroupCustom("...")
    MyBaseExceptionGroup("...")
    MyBaseExceptionGroupCustom("...")

```

and you can run this to check the PR:
```sh
target/debug/ruff check tmp/test_error.py --select PLW0133 --unsafe-fixes --diff --no-cache --isolated --target-version py310
target/debug/ruff check tmp/test_error.py --select PLW0133 --unsafe-fixes --diff --no-cache --isolated --target-version py314
```

---

_Renamed from "improvement: detect custom exception in useless_expression_statement" to "improvement: detect custom exceptions in useless_expression_statement" by @LoicRiegel on 2025-11-11 15:23_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 15:33_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/useless_exception_statement.rs`:56 on 2025-11-17 19:40_

I think we should make this a preview change since the rule itself is stable. The ecosystem report was clear, but I feel like this could still apply to a lot more situations.

You can see [`preview.rs`](https://github.com/astral-sh/ruff/blob/c0fb235a703684bca1cc1767015bb7e19ab1c22f/crates/ruff_linter/src/preview.rs#L16-L19) for some examples.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/useless_exception_statement.py`:6 on 2025-11-17 19:45_

Should we try one test case with another link in the inheritance chain like:

```py
class MySubError(MyError): ...
```

I think `any_base_class` is recursive, so we'll handle that too.

---

_@ntBre reviewed on 2025-11-17 19:51_

Thank you! This looks great overall. I just think we should make it a preview change and maybe add a recursive test case (which I think your implementation will already handle well).

---

_Label `rule` added by @ntBre on 2025-11-17 19:51_

---

_Label `preview` added by @ntBre on 2025-11-17 19:51_

---

_Renamed from "improvement: detect custom exceptions in useless_expression_statement" to "[`pylint`] improve `PLW0133` to detect custom exceptions in useless_expression_statement" by @amyreese on 2025-11-17 22:18_

---

_Comment by @LoicRiegel on 2025-11-28 17:59_

Thank you for the review! No problem to do the changes, I've just been very busy with work, but I'll find the time to finish this PR in the next few days :)

---

_@LoicRiegel reviewed on 2025-11-30 18:08_

---

_Review comment by @LoicRiegel on `crates/ruff_linter/resources/test/fixtures/pylint/useless_exception_statement.py`:6 on 2025-11-30 18:08_

Done, yes this was already handled well

---

_Review comment by @LoicRiegel on `crates/ruff_linter/src/rules/pylint/rules/useless_exception_statement.rs`:56 on 2025-11-30 18:13_

- I added a new function with the link of this PR in preview.rs
- documentation updated

⚠️ now the cargo insta snapshots don't detect the new test cases I added. They don't run with preview enabled, do they? How should I fix that ?

---

_@LoicRiegel reviewed on 2025-11-30 18:13_

---

_@ntBre reviewed on 2025-12-01 14:34_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/useless_exception_statement.rs`:56 on 2025-12-01 14:34_

I would just add a new `preview_rules` function that's a copy of `rules`:

https://github.com/astral-sh/ruff/blob/d7b2d261da5b03d211e1c19b4a49e3451454082b/crates/ruff_linter/src/rules/pylint/mod.rs#L237-L239

with preview enabled and `preview` added in the snapshot name. Here's a random example from another rule:

https://github.com/astral-sh/ruff/blob/d7b2d261da5b03d211e1c19b4a49e3451454082b/crates/ruff_linter/src/rules/flake8_bugbear/mod.rs#L97-L103

or we could use the relatively new `assert_diagnostics_diff` macro:

https://github.com/astral-sh/ruff/blob/6e7a2dce0fbccd749f4dc503a50b61bc50d88b80/crates/ruff_linter/src/rules/flake8_bandit/mod.rs#L116-L127

---

_@LoicRiegel reviewed on 2025-12-06 12:07_

---

_Review comment by @LoicRiegel on `crates/ruff_linter/src/rules/pylint/rules/useless_exception_statement.rs`:56 on 2025-12-06 12:07_

Thank you very much for the hint! ``assert_diagnotics_diff`` is really nice! I used and now both snapshots are good!
I also indicated in the fixture file which lines are supposed to be caught only in preview

Hope everything is alright now :)

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/useless_exception_statement.py`:27 on 2025-12-08 19:09_

Should we remove these `(review)` suffixes? Happy to delete them myself, I was just curious if they were needed for some reason.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/useless_exception_statement.rs`:20 on 2025-12-08 19:13_

I think we should probably keep the `## Known problems` heading and the old text, with the new preview behavior following it. There will still be a known problem, even in preview, in that we can't resolve user-defined exceptions defined in other files.

---

_@ntBre reviewed on 2025-12-08 19:23_

Thanks! Just a couple more small suggestions.

---

_@LoicRiegel reviewed on 2025-12-08 22:11_

---

_Review comment by @LoicRiegel on `crates/ruff_linter/resources/test/fixtures/pylint/useless_exception_statement.py`:27 on 2025-12-08 22:11_

I thought those comments made it a little easier when reviewing cargo insta snapshots, but I totally understand if you prefer not to have them, it'll be one less thing to worry about when making the preview behavior stable.
I did it in a specific commit on purpose, so I'll just revert it :)

---

_@LoicRiegel reviewed on 2025-12-08 22:21_

---

_Review comment by @LoicRiegel on `crates/ruff_linter/src/rules/pylint/rules/useless_exception_statement.rs`:20 on 2025-12-08 22:21_

Oh right, it won't work if the exception is define in a different file.
Yes I'll put back the Known problem section, and maybe include in the Preview section that it only works for exceptions defined in the file being checked.

Waw this makes me think, custom exceptions are usually imported and raised in lots of files, mist of the time not the file where they are defined... This PR is still an improvement, but with limited impact... 
Maybe we should keep the issue open...

Is there a way I could continue this work and make it work for custom exceptions in different files?

---

_@ntBre reviewed on 2025-12-08 22:27_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/useless_exception_statement.py`:27 on 2025-12-08 22:27_

Ohhh, are they supposed to say `preview` instead of `review`? That does make some sense to me! I thought they were related to the code review, but it may have just been a typo.

---

_@ntBre reviewed on 2025-12-08 22:30_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/useless_exception_statement.rs`:20 on 2025-12-08 22:30_

Yeah, this is why the issue is labeled `type-inference`. That label used to be `multi-file-analysis`, but we combined them since our type checker has both. Ruff currently can't do any multi-file analysis, so this is the best we can do for now. I think that means it's okay to close the issue too.

This is still a nice improvement, as you said!

---

_@LoicRiegel reviewed on 2025-12-08 23:32_

---

_Review comment by @LoicRiegel on `crates/ruff_linter/resources/test/fixtures/pylint/useless_exception_statement.py`:27 on 2025-12-08 23:32_

Oh my god yes I meant to write "preview*! So sorry for the typo 
Okay, so let me know if you want me to revert, otherwise I'll fix the typo :)

---

_@ntBre reviewed on 2025-12-08 23:41_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/useless_exception_statement.py`:27 on 2025-12-08 23:41_

I'd probably still lean toward reverting now that we're using `assert_diagnostics_diff`, but either works for me.

---

_Review comment by @LoicRiegel on `crates/ruff_linter/src/rules/pylint/rules/useless_exception_statement.rs`:20 on 2025-12-09 09:11_

I updated the documentation like we said

---

_@LoicRiegel reviewed on 2025-12-09 09:11_

---

_@LoicRiegel reviewed on 2025-12-09 09:11_

---

_Review comment by @LoicRiegel on `crates/ruff_linter/resources/test/fixtures/pylint/useless_exception_statement.py`:27 on 2025-12-09 09:11_

Absolutely, it's reverted :)

---

_@ntBre approved on 2025-12-09 18:47_

Thank you!

---

_Renamed from "[`pylint`] improve `PLW0133` to detect custom exceptions in useless_expression_statement" to "[`pylint`] Detect subclasses of builtin exceptions (`PLW0133`)" by @ntBre on 2025-12-09 18:49_

---

_Merged by @ntBre on 2025-12-09 18:49_

---

_Closed by @ntBre on 2025-12-09 18:49_

---

_Branch deleted on 2025-12-10 07:42_

---
