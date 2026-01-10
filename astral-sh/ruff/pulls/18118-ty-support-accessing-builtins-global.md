```yaml
number: 18118
title: "[ty] support accessing `__builtins__` global"
type: pull_request
state: merged
author: felixscherz
labels:
  - ty
assignees: []
merged: true
base: main
head: fix/support-for-__builtins__
created_at: 2025-05-15T10:53:26Z
updated_at: 2025-05-15T20:01:38Z
url: https://github.com/astral-sh/ruff/pull/18118
synced_at: 2026-01-10T18:51:01Z
```

# [ty] support accessing `__builtins__` global

---

_Pull request opened by @felixscherz on 2025-05-15 10:53_

Hi, this is in regards to https://github.com/astral-sh/ty/issues/393 and intends to support `__builtins__` access.

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

The PR adds an explicit check for `"__builtins__"` during name lookup, similar to how `"__file__"` is implemented. The inferred type is `ModuleType`.

I'm new to working on ty, so any feedback is welcome:)

## Test Plan

Added a markdown test for `__builtins__`.


---

_Review requested from @carljm by @felixscherz on 2025-05-15 10:53_

---

_Review requested from @AlexWaygood by @felixscherz on 2025-05-15 10:53_

---

_Review requested from @sharkdp by @felixscherz on 2025-05-15 10:53_

---

_Review requested from @dcreager by @felixscherz on 2025-05-15 10:53_

---

_Label `ty` added by @MichaReiser on 2025-05-15 10:58_

---

_Comment by @github-actions[bot] on 2025-05-15 11:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
- error[unresolved-reference] tests/test_make.py:1823:24: Name `__builtins__` used when not defined
- Found 618 diagnostics
+ Found 617 diagnostics

trio (https://github.com/python-trio/trio)
- error[unresolved-reference] src/trio/_tests/test_repl.py:47:29: Name `__builtins__` used when not defined
- Found 1097 diagnostics
+ Found 1096 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[unresolved-reference] scrapy/spiderloader.py:37:52: Name `__builtins__` used when not defined
- Found 1423 diagnostics
+ Found 1422 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ warning[unused-ignore-comment] ddtrace/debugging/_expressions.py:79:61: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] ddtrace/debugging/_expressions.py:199:38: Unused blanket `type: ignore` directive
- Found 8729 diagnostics
+ Found 8731 diagnostics

```
</details>


---

_Comment by @sharkdp on 2025-05-15 11:16_

Thank you for your contribution! You found the right place to add the `name == "__builtins__"` check and the right place for the test. Looking at the ecosystem impact, it would be great if we could infer the actual `Type::ModuleLiteral(…)` type. Doing so would probably require adding something like a `KnownModule::to_module_literal_type(db, …)` function. You could then call `KnownModule::Builtins.to_module_literal_type(db, …)`. The hardest part might be to get the right value for the `importing_file` when calling `ModuleLiteralType::new`.

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/src/symbol.rs`:1017 on 2025-05-15 13:38_

To quote the docs:

> As an implementation detail, most modules have the name `__builtins__` made available as part of their globals. The value of `__builtins__` is normally either this module or the value of this module’s [`__dict__`](https://docs.python.org/3/reference/datamodel.html#object.__dict__) attribute. Since this is an implementation detail, it may not be used by alternate implementations of Python.

As such, the type of `__builtins__` is, as David has pointed out, the `builtins` module, and the symbol's boundness should be `PossiblyUnbound`.

---

_Review comment by @InSyncWithFoo on `crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:29 on 2025-05-15 13:38_

As per the previous review comment, these tests should be added as well:

```python
import sys
reveal_type(sys.__builtins__)  # revealed: <module 'builtins'>

from builtins import __builtins__ as __bi__
reveal_type(__bi__)  # revealed: <module 'builtins'>
```

---

_@InSyncWithFoo reviewed on 2025-05-15 13:39_

---

_Comment by @InSyncWithFoo on 2025-05-15 13:49_

> The hardest part might be to get the right value for the `importing_file` when calling `ModuleLiteralType::new`.

This is actually not that hard: all call sites of `module_type_implicit_global_symbol()` have access to `File`s as well.

* `global_symbol()`: `file` parameter.
* `builtins_symbol()`: `module.file()`.
* The two references in `infer.rs`: `self.file()`.

As for `some_module.__builtins__`, a branch could probably be added to `member_lookup_with_policy()`.


---

_Comment by @carljm on 2025-05-15 15:00_

I notice that one of the ecosystem hits is using `__builtins__` as if it were a dictionary, not a module instance. The docs quoted by @InSyncWithFoo call out that it can be either. And a simple test shows this to be true: with the code `print(type(__builtins__))` in `main.py`, then `python3 main.py` prints `<class 'module'>`, but `python3 -c "import main"` prints `<class 'dict'>`.

So this suggests to me that we must either infer the type of `__builtins__` as a union (and require users of it to check which it is) or just follow pyright's lead and infer it as `Any`. Given that this pattern is uncommon, and it would be quite complex to make the stricter version fully strict (it would require the dictionary variant of the union to be a TypedDict with known keys and values), I think we should just take the easier route and infer it as `Any`.

---

_Comment by @AlexWaygood on 2025-05-15 15:41_

(I agree with @carljm -- let's just use `Any` for now. We can always revisit it and try to improve the type at a later date if we feel it's important.)

---

_@felixscherz reviewed on 2025-05-15 19:20_

---

_Review comment by @felixscherz on `crates/ty_python_semantic/resources/mdtest/scopes/moduletype_attrs.md`:29 on 2025-05-15 19:20_

I added tests for these cases.

---

_@sharkdp reviewed on 2025-05-15 19:38_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/symbol.rs`:1019 on 2025-05-15 19:38_

You can use
```suggestion
            Symbol::bound(Type::any()).into()
```
here

---

_@sharkdp reviewed on 2025-05-15 19:39_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/symbol.rs`:328 on 2025-05-15 19:39_

You can use
```suggestion
                Symbol::bound(Type::any()).into()
```
here

---

_@sharkdp approved on 2025-05-15 19:40_

Thank you for the updates!

---

_Comment by @sharkdp on 2025-05-15 20:00_

The mypy_primer results also look as expected now.

---

_Merged by @sharkdp on 2025-05-15 20:01_

---

_Closed by @sharkdp on 2025-05-15 20:01_

---
