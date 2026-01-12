```yaml
number: 3302
title: "Add flake-pyi PYI033 \"Do not use type comments in stubs\""
type: pull_request
state: merged
author: konstin
labels:
  - rule
assignees: []
merged: true
base: main
head: flake8-pyi-Y033-type-comments-in-stub-files
created_at: 2023-03-02T12:18:21Z
updated_at: 2023-03-03T15:50:32Z
url: https://github.com/astral-sh/ruff/pull/3302
synced_at: 2026-01-12T15:55:12Z
```

# Add flake-pyi PYI033 "Do not use type comments in stubs"

---

_@konstin_

I think this is also a case where people like want this for .py files, too, at least on 3.7+ (https://docs.python.org/3/whatsnew/3.7.html#pep-563-postponed-evaluation-of-annotations)

On that note, does none of the tools upgrade all type comments to type annotations automatically? I looked in pyupgrade and googled and was surprised i couldn't find anything

---

_Review requested from @charliermarsh by @konstin on 2023-03-02 12:18_

---

_Review comment by @konstin on `docs/configuration.md`:209 on 2023-03-02 14:40_

not sure how this got in here, should this be here?

---

_@konstin reviewed on 2023-03-02 14:40_

---

_@twoertwein reviewed on 2023-03-02 14:53_

---

_Review comment by @twoertwein on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:46 on 2023-03-02 14:53_

Would this also be true for `# type: ignore`?

---

_@konstin reviewed on 2023-03-02 14:56_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:46 on 2023-03-02 14:56_

currently yes; are there any cases where you need `# type: ignore` in a .pyi file?

---

_@twoertwein reviewed on 2023-03-02 15:05_

---

_Review comment by @twoertwein on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:46 on 2023-03-02 15:05_

I wish ignore comments wouldn't be needed in stubs but they are used. Here is an example from typeshed https://github.com/python/typeshed/blob/45f0a5e7e47a23d6be67cfce429c4f2c0de62a29/stdlib/builtins.pyi#L565

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:46 on 2023-03-02 15:12_

thanks for the example! i'll except those then

---

_@konstin reviewed on 2023-03-02 15:12_

---

_@AlexWaygood reviewed on 2023-03-02 15:43_

---

_Review comment by @AlexWaygood on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:46 on 2023-03-02 15:43_

All our flake8-pyi tests for this rule are in this file FYI, if that helps :)

https://github.com/PyCQA/flake8-pyi/blob/main/tests/type_comments.pyi

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:46 on 2023-03-02 15:44_

It might be worth looking at the `flake8-pyi` source and seeing how they handle this?

In particular, they have this regex, which omits `ignore` comments:

https://github.com/PyCQA/flake8-pyi/blob/66f28a4407af2156f95d370941fae4dd98280740/pyi.py#L1877

---

_@charliermarsh reviewed on 2023-03-02 15:44_

---

_@konstin reviewed on 2023-03-02 15:47_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:46 on 2023-03-02 15:47_

oops thanks i somehow used them for Y006 but then forgot that those also exist for Y033.

thanks!

---

_Comment by @charliermarsh on 2023-03-02 15:51_

> On that note, does none of the tools upgrade all type comments to type annotations automatically? I looked in pyupgrade and googled and was surprised i couldn't find anything

There's a tool called [com2ann](https://github.com/ilevkivskyi/com2ann) which seems popular for this use-case. It's come up in Ruff before, though I've tried to cut scope at "tools that support one-time 2-to-3 conversion" :)

---

_@charliermarsh reviewed on 2023-03-02 16:09_

---

_Review comment by @charliermarsh on `docs/configuration.md`:209 on 2023-03-02 16:09_

Oh strange. Builds should fail if `cargo dev generate-all` produces any changed files. Lemme look into it.

---

_@konstin reviewed on 2023-03-02 16:41_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:46 on 2023-03-02 16:41_

fwiw i had to do two regexes since rust regex doesn't support look around

---

_Comment by @JonathanPlasse on 2023-03-02 16:49_

Maybe `com2ann` should be mentioned in the rule explanation.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:16 on 2023-03-02 17:55_

I think this paragraph would make sense in the `## Why is it bad?` section, since `## What it does?` tends to be a description of what it's checking rather than alternatives etc. So e.g.:

```rs
/// ## What it does
/// Checks for the use of type comments (e.g., `x = 1  # type: int`) in stub
/// files.
///
/// ## Why is this bad?
/// Stub (`.pyi`) files should always use type annotations directly, rather
/// than type comments, even if they're intended to support Python 2, since
/// stub files are not executed at runtime.
///
/// ## Example
/// ```python
/// x = 1 # type: int
/// ```
///
/// Use instead:
/// ```python
/// x: int = 1
/// ```
```



---

_@charliermarsh approved on 2023-03-02 17:55_

LGTM! One suggestion on the docstring.

---

_@charliermarsh reviewed on 2023-03-02 17:55_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:46 on 2023-03-02 17:55_

Thanks @AlexWaygood for the helpful comment :)

---

_@konstin reviewed on 2023-03-03 10:11_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:16 on 2023-03-03 10:11_

thanks!

---

_@konstin reviewed on 2023-03-03 10:11_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_pyi/rules/type_comment_in_stub.rs`:16 on 2023-03-03 10:11_

i've also added the `# type: ignore` exception

---

_Merged by @charliermarsh on 2023-03-03 15:45_

---

_Closed by @charliermarsh on 2023-03-03 15:45_

---

_Label `rule` added by @charliermarsh on 2023-03-03 15:45_

---

_Branch deleted on 2023-03-03 15:50_

---
