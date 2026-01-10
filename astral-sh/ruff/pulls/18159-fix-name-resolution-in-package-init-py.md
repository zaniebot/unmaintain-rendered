```yaml
number: 18159
title: Fix name resolution in package __init__.py
type: pull_request
state: open
author: robsdedude
labels:
  - preview
  - do-not-merge
  - great writeup
assignees: []
base: main
head: fix/resolve-names-in-package-init
created_at: 2025-05-17T22:27:32Z
updated_at: 2025-07-25T06:59:44Z
url: https://github.com/astral-sh/ruff/pull/18159
synced_at: 2026-01-10T17:58:13Z
```

# Fix name resolution in package __init__.py

---

_Pull request opened by @robsdedude on 2025-05-17 22:27_

## Summary
When resolving the name while linting a package's `__init__.py` file, ruff was considering `__init__` to be a proper module name. Whereas it's not really. Classes, functions, and other objects defined in `__init__.py` will belong to the package. This PR fixes that resolution. In particular, `D103`, and `PLW0406` were affected. Maybe more that I couldn't find.

## Test Plan
I've added tests for the two rules I could find were affected by not properly resolving qualified names of objects inside a package's `__init__.py`.


---

_Review requested from @dylwil3 by @MichaReiser on 2025-05-18 16:13_

---

_Comment by @github-actions[bot] on 2025-05-19 13:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/3e70ace0292744d1210c3488b6577be2b54d2075/ibis/__init__.py#L78'>ibis/__init__.py:78:12:</a> PLW0406 Module `ibis` imports itself
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0406 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/3e70ace0292744d1210c3488b6577be2b54d2075/ibis/__init__.py#L78'>ibis/__init__.py:78:12:</a> PLW0406 Module `ibis` imports itself
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0406 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @dylwil3 on 2025-05-19 22:31_

Thanks for finding this!

I think it would be good to understand better what the correct fix is here. In the semantic model, `self.module.qualified_name()` is used in quite a few spots. Is there a particular reason why we would only need to change the output of that method in this one place? Should we make a new method that captures this different usage? Should the output just _always_ omit `__init__.py`?

I would have to dive into this a bit to answer those questions but I'm curious to know if you've already considered some of these. (No worries if not!)

And certainly we would need to add more tests - I think that method is called on a class or function in many different places. One reassurance is that the ecosystem check showed no changes, but we should still do some due diligence.

Finally, depending on the change, and whether it should always be considered a bug fix, we may want to consider if it's possible to gate this behavior behind preview.

---

_Comment by @robsdedude on 2025-05-20 07:44_

> I would have to dive into this a bit to answer those questions but I'm curious to know if you've already considered some of these. (No worries if not!)

Great question. I should've probably mentioned that in the PR description. I diligently went over all 270 usages of `SemanticMode::resolve_qualified_name` my IDE found (and again now to compile the below info :sweat_smile:).

Almost all of them are in one shape or another only matching against a hard-coded identifier external to the code base being analyzed. E.g,
```rust
 match qualified_name.segments() {
            ["collections", "namedtuple"]  => ...
}
```
or 
```rust
self.semantic
    .resolve_qualified_name(func)
    .and_then(|qualified_name| {
        if self
            .semantic
            .match_typing_qualified_name(&qualified_name, "cast")
        { ... }
        else { ... }
    })
```
For all of those, this change would me no difference except for if the code base contains a module that shadows these names.

<details>

<summary>Example</summary>

With and without this patch:

```python
# file: typing.py
def cast(): ...  # ruff resolves this to qualified name `typing.cast`
```

Only with this patch:
```python
# file: typing/__init__.py
def cast(): ...  # ruff resolves this to qualified name `typing.cast`
```

</details>


## Exceptions to this I found:

### Ones similar to `settings.pydocstyle.ignore_decorators` for which I've added a test:
 * `unsafe_markup_use.rs` compares to `settings.flake8_bandit.allowed_markup_calls`
 * the same file similarly compares to `settings.flake8_bandit.extend_markup_names`
 * `flake8_boolean_trap/helpers.rs` similarly compares against `settings.flake8_boolean_trap.extend_allowed_calls`
 * `flake8_pytest_style/rules/raises.rs` checks against glob patterns in `settings.flake8_pytest_style.raises_require_match_for` and  `settings.flake8_pytest_style.raises_extend_require_match_for`
 * `flake8_pytest_style/rules/warns.rs` matches against `settings.flake8_pytest_style.warns_require_match_for` and `...warns_extend_require_match_for`
 * `flake8_tidy_imports/rules/banned_api.rs` matches against `settings.flake8_tidy_imports.banned_api`
 * `flake8_type_checking/helpers.rs` checks against `settings.flake8_type_checking.runtime_required_decorators`
 * `settings.flake8_type_checking.runtime_required_base_classes` is checked against indirectly through `flake8_type_checking::helpers::runtime_required_class` calling `runtime_required_base_class` calling `any_qualified_base_class` calling `any_base_classany_base_class`, finally calling `resolve_qualified_name`
 * `function_call_in_argument_default` in `flake8_bugbear/rules/function_call_in_argument_default.rs` passes `settings.flake8_bugbear.extend_immutable_calls` down the stack to finally be matched against the result of `resolve_qualified_name`.

For all of those settings, I think resolving names in `__init__.py`s to be part of the parent module improves the clarity of the config options like it does for `pydocstyle.ignore_decorators`. Admittedly, there is a small risk of this breaking users that have configures some `...__init__.my_identifier` for one of these config options. Side note: for some of these config options, I don't think there's a reasonable use case to add entries belonging to a user-module. For those, the impact of this PR is negligible.

### Other
 * `BodyVisitor::visit_stmt` in `pydoclint/rules/check_docstring.rs` uses it to resolve exceptions being raised to check that the doc string mentions these.

I'd argue that this PR also improves this situation (test should be written to verify), as you don't want to have to document an exception defined in an `__init__.py` file as `Raises my_package.__init__.MyError`.

 * In some places, the qualified name is used in the reported diagnostic. So this change would alter the output of ruff in such cases. E.g.,
   * `future_rewritable_type_annotation`

Here, I think the PR might make things ever so slightly worse, by changing the output to point to `my_package.some_problematic_place` instead of `my_package.__init__.some_problematic_place`
### Unclear impact
 * `AttributeSearcher` in `numpy/helper.rs` uses `resolve_qualified_name`. Looking at its usages, I'm not 100% sure about the implications here. I'll leave that to you :innocent:
 * `Callee::try_from_call_expression` in `pylint/rules/unspecified_encoding.rs` uses it. I think I've managed to track all usages and it also only leads to matches against hard-coded external identifiers. But again, I'd ask you to double-check, as this code is less straight forward to follow.
 * `ruff_python_semantic/src/analyze/typing.rs::resolve_assignment` calls the function.
   * If I understand it correctly, the PR won't change anything here either, because analysis is limited to a single file. So when resolving an assignment, when before the assignee was names `foo.__init__.bar` throughout the file, it's now simply called `foo.bar`throughout the file. N.B. again, if `foo` happens to be some other shadowed package (e.g., `typing`) this could actually make a difference.

---

_Label `great writeup` added by @dylwil3 on 2025-05-20 12:55_

---

_Comment by @MichaReiser on 2025-06-20 06:39_

@dylwil3 could you take another look at this PR?

---

_Comment by @dylwil3 on 2025-07-11 21:02_

@robsdedude Sorry for the long delay - this was trickier than I expected to sort out. I have made the following changes, and I was hoping you could look them over to see if you agree.

1. The main difference in behavior is that I _believe_ that in your new fixture for [import-self (PLW0406)](https://docs.astral.sh/ruff/rules/import-self/#import-self-plw0406) we should _not_ emit a lint. I tried making a package with the layout you had there and playing around with it and as far as I can tell there is not a circular import error. But I would not be surprised to learn I was wrong - would it be possible for you to outline what steps I should take to actually get a circular import error from that setup?
2. Assuming (1) is correct, I believe the implementation can be simplified and put in a possibly more logical place. I believe the `qualified_name` method itself should just skip `__init__` in the path, rather than implementing this logic once we get to the semantic model. This requires some custom logic to be added to the resolution of relative imports. But as far as I can tell that's where that logic belongs - since that more closely models what Python itself does when it sees a relative import: the first leading `.` turns into "find the package containing this module". If the module is the package root (e.g. `foo/__init__.py`)  that would be the parent directory (i.e. `foo` here).

If this makes sense to you then the final challenge will be finding a way to gate this behind preview, since it will be a breaking change for various configurations/settings, as you pointed out.

Let me know what you think, and thanks for your contribution!

---

_Label `do-not-merge` added by @dylwil3 on 2025-07-11 21:04_

---

_Label `preview` added by @dylwil3 on 2025-07-11 21:04_

---

_Comment by @robsdedude on 2025-07-24 21:26_

Sorry for the late reply, I have fairly many things going on in life currently, so my availability for OSS contributions is unfortunately pretty limited. This will likely continue for a while just as a heads-up. I'm still interested and happy to contribute as time permits.

I dug deeper and am left more confused than before... The rule docs sure make it seem that the following set up should fail with an `ImportError`:
```python
# ~/mod.py
import mod
```
```bash
~ $ python -c "import mod"
```

It doesn't. Neither does this fail:
```python
# ~/package/__init__.py exists and is empty
# this file: ~/package/mod.py
from . import mod
```
```bash
~ $ python -c "from package import mod"
```

I tested it with Python 3.3 (oldest 3 I could get running without too much fiddling) and 3.13. Both are fine at runtime, yet the second at least triggers the lint. Can you shed some light on this, please? 


---
