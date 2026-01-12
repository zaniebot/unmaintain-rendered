```yaml
number: 15719
title: "Recognize all symbols named `TYPE_CHECKING` for `in_type_checking_block`"
type: pull_request
state: merged
author: Daverball
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/TYPE_CHECKING_without_typing_import
created_at: 2025-01-24T14:56:31Z
updated_at: 2025-02-06T14:03:49Z
url: https://github.com/astral-sh/ruff/pull/15719
synced_at: 2026-01-12T15:55:52Z
```

# Recognize all symbols named `TYPE_CHECKING` for `in_type_checking_block`

---

_@Daverball_

Closes #15681

## Summary

This changes `analyze::typing::is_type_checking_block` to recognize all symbols named "TYPE_CHECKING".
This matches the current behavior of mypy and pyright as well as `flake8-type-checking`.

It also drops support for detecting `if False:` and `if 0:` as type checking blocks. This used to be an option for
providing backwards compatibility with Python versions that did not have a `typing` module, but has since
been removed from the typing spec and is no longer supported by any of the mainstream type checkers.

## Test Plan

`cargo nextest run`


---

_Comment by @Daverball on 2025-01-24 15:02_

I will add more test cases, since I'm worried some of the TC fixes will not be able to deal with these new kinds of type checking blocks without additional changes.

---

_Comment by @github-actions[bot] on 2025-01-24 15:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/54f9a8989cf20fc0cace4537702acf57ae8502ec/src/scikit_build_core/resources/_editable_redirect.py#L11'>src/scikit_build_core/resources/_editable_redirect.py:11:12:</a> TC004 Move import `importlib.machinery` out of type-checking block. Import is used for more than type hinting.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| TC004 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2025-01-24 15:09_

That was quick, thanks. I'm leaning towards making this a preview change because it's technically breaking. The counterargument is that there's barely any ecosystem change.

---

_Label `rule` added by @MichaReiser on 2025-01-24 15:09_

---

_Comment by @Daverball on 2025-01-24 15:16_

I'm a little confused by the scikit ecosystem change. If anything I would've expected a previous TC003 error to disappear, not a new TC004 to appear, especially since there is no runtime use of `importlib.machinery` in that file...

So it seems like there's a different bug at play here, that was previously obscured  by not recognizing the `TYPE_CHECKING` block in that file.

---

_Comment by @Daverball on 2025-01-24 15:23_

@MichaReiser If we're going to treat it as a breaking change anyways and put it behind the preview flag, I would suggest to also stop treating `if False` and `if 0` as type checking blocks in the same change, since that hasn't been a thing since type checkers dropped support for Python 2.7/3.5.

---

_Comment by @Daverball on 2025-01-24 16:04_

> I'm a little confused by the scikit ecosystem change. If anything I would've expected a previous TC003 error to disappear, not a new TC004 to appear, especially since there is no runtime use of `importlib.machinery` in that file...
> 
> So it seems like there's a different bug at play here, that was previously obscured by not recognizing the `TYPE_CHECKING` block in that file.

Looks like references to bindings don't really try to find their matching import beyond the first level, so since there are multiple `importlib` bindings, the references to `importlib.abc` and `importlib.util` will bind to the most recent `importlib` import, which happens to be `importlib.machinery` in the type checking block.

So it looks like `TC001-004` will need to do something more sophisticated than simply looking at import bindings and their references, when that import binding came from a dotted import and it's shadowing another import. I will open a separate issue for this problem.

---

_Comment by @Daverball on 2025-01-25 15:09_

@MichaReiser I wonder if it would be alright to copy the preview flag to the `SemanticModel`. `in_type_checking_block` is used in quite a few places, so adding a new argument for the preview behavior only to remove it again later when we stabilize this, would end up introducing quite a bit of churn. Adding the flag to the `SemanticModel` would be a much smaller change and I would've found it quite useful in the past, just like `checker.source_type` is duplicated into a semantic flag for stub files.

I will also need to make some structural changes to `Importer::typing_import_edit` so it can still emit a fix for these new type checking blocks, since it currently always assumes it will need an import of `TYPE_CHECKING` from one of the typing modules, but we know for sure it will not need that if we have a preceding type checking block. If we want to gate this change behind a preview flag as well, that would be another place where having the preview flag on the `SemanticModel` would be useful.

---

_@Daverball reviewed on 2025-01-26 14:16_

---

_Review comment by @Daverball on `crates/ruff_linter/src/importer/mod.rs`:170 on 2025-01-26 14:16_

By restructuring the logic, this no-op edit would now only happen if we don't have an existing preceding type checking block. I'm not sure whether this is problematic or why we're adding a no-op edit to begin with (maybe to ensure no other edit is allowed to remove the import?).

If this no-op edit indeed accomplishes something useful, we may need a more sophisticated change, which allows `find_type_checking` to return a name based on an assignment target in addition to an import. This would allow us to write separate fix logic for when it's imported and when it's defined in the same file.

---

_Review requested from @carljm by @MichaReiser on 2025-02-04 10:13_

---

_Review requested from @MichaReiser by @MichaReiser on 2025-02-04 10:13_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-04 10:13_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-04 10:13_

---

_Review request for @carljm removed by @AlexWaygood on 2025-02-04 10:15_

---

_Review request for @sharkdp removed by @AlexWaygood on 2025-02-04 10:15_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-02-04 10:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:353 on 2025-02-04 10:16_

Nit:
```suggestion
            id == "TYPE_CHECKING"
            // Ex) `if TC:` with `from typing import TYPE_CHECKING as TC`
            || semantic.match_typing_expr(test, "TYPE_CHECKING")
        }
        // Ex) `if typing.TYPE_CHECKING:`
        Expr::Attribute(ast::ExprAttribute { attr, .. }) => attr == "TYPE_CHECKING",
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/importer/mod.rs`:151 on 2025-02-04 10:17_

Hmm, yeah this is interesting. Not having an option seems fine to me for now, we can introduce one later if it is being asked for.

---

_@MichaReiser approved on 2025-02-04 10:17_

This looks great. However, this is a somewhat significant change. I think i'd feel better if we gate this behind the preview mode and release it as part of 0.10. 



---

_Comment by @Daverball on 2025-02-04 10:59_

> This looks great. However, this is a somewhat significant change. I think i'd feel better if we gate this behind the preview mode and release it as part of 0.10.

The only problematic part of that is adding a preview flag check will add quite the rat tail to the diff, since the function is used in so many places. One way to get around this would be to add the preview flag to the semantic model. Which I would've found useful in the past anyways. (Either as a `SemanticModelFlag` or its own thing)

---

_Comment by @MichaReiser on 2025-02-04 11:02_

Ah right, sorry. I forgot about our previous conversation while I was on PTO. Let me have a quick look

---

_Comment by @MichaReiser on 2025-02-04 11:05_

Adding a preview option to `SemanticModel` or to `SemanticModelFlag` sounds reasonable to me. The alternative is to add a flag specific for this feature to `SemanticModelFlag`. The advantage of a feature-specific flag is that it's easier to promote all changes because we can simply remove the flag and then fix all compile errors.

---

_Label `preview` added by @MichaReiser on 2025-02-04 13:26_

---

_Comment by @Daverball on 2025-02-04 13:27_

@MichaReiser Since we're now gating the change I also removed special casing for `if False:` and `if 0:` in preview mode.
I think support for this compatibility shim has been dropped 4+ years ago.

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/typing.rs`:385 on 2025-02-04 13:28_

I think this version is now slightly different to what you had before as it removes support for `if False:` and `if 0:`. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/importer/mod.rs`:138 on 2025-02-04 13:30_

Do we need to cate this behind preview? I understood that this was mainly a refactor that moved the `preceding_type_checking_block` to the top to avoid unnecessarily going through the `Import the TYPE_CHECKING symbol` when it isn't necessary

---

_@MichaReiser reviewed on 2025-02-04 13:30_

---

_Comment by @MichaReiser on 2025-02-04 13:31_

> @MichaReiser Since we're now gating the change I also removed special casing for `if False:` and `if 0:` in preview mode. I think support for this compatibility shim has been dropped 4+ years ago.

Sounds reasonable. Would you mind updating your PR summary to reflect this change?



---

_@Daverball reviewed on 2025-02-04 13:35_

---

_Review comment by @Daverball on `crates/ruff_linter/src/importer/mod.rs`:138 on 2025-02-04 13:35_

Look at my previous comment here: https://github.com/astral-sh/ruff/pull/15719#discussion_r1929786168

Refactoring has the side effect of never emitting a no-op fix for that branch. So I feel more safe doing it this way, until I know what the purpose of the no-op fix was.

---

_@MichaReiser reviewed on 2025-02-04 13:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/importer/mod.rs`:170 on 2025-02-04 13:51_

Oh yeah, the no-op edit is important. It's a "hack" to prevent any other fix to remove the import. At least, I remember something along those lines from @charliermarsh 

---

_@Daverball reviewed on 2025-02-04 14:09_

---

_Review comment by @Daverball on `crates/ruff_linter/src/importer/mod.rs`:170 on 2025-02-04 14:09_

I assumed that was the case, but then was surprised that none of the tests were failing. Seems kind of important to have at least one regression test for this case. In the absence of a failing test the other possibility is that this since has been fixed in a different way and is now essentially doing nothing.

---

_@Daverball reviewed on 2025-02-04 14:17_

---

_Review comment by @Daverball on `crates/ruff_linter/src/importer/mod.rs`:170 on 2025-02-04 14:17_

The other thing that bothered me, is that this hack does not seem very reliable, since the type checking block we're moving the imports to might not correspond to the `typing`/`TYPE_CHECKING` import we find in the first step...

Maybe we revert it back to my earlier refactored version, but when we have an existing type checking block we lookup the relevant statement from the binding that's referenced by the condition in the type checking block and emit a no-op edit for that specific statement if it doesn't overlap with one of the import edits. That seems a lot more robust to me.

---

_@Daverball reviewed on 2025-02-05 13:04_

---

_Review comment by @Daverball on `crates/ruff_linter/src/importer/mod.rs`:170 on 2025-02-05 13:04_

I had another little think about this. I don't think we have to worry about the no-op edit when we find a preceding type checking block. If another edit would remove the binding we're referencing, it would necessarily also have to remove the type checking block itself (otherwise there would still be a reference to the binding), but then the edit that removes the type checking block would overlap with our edit to add a new import to it, so we don't need to artificially create another overlap.

In the case where there isn't a type checking block, but there is a unused `TYPE_CHECKING` import, it makes sense, since we don't want to let F401 remove the import when our edit adds a new reference to it.

---

_@charliermarsh reviewed on 2025-02-05 15:26_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/importer/mod.rs`:170 on 2025-02-05 15:26_

Yeah, I believe this is to prevent things like:

```python
from typing import TYPE_CHECKING

from foo import Bar

def func() -> Bar: ...
```

If we don't add the no-op edit, `TYPE_CHECKING` might be removed, leaving us with:

```python
if TYPE_CHECKING:
  from foo import Bar

def func() -> "Bar": ...
```

---

_@Daverball reviewed on 2025-02-06 08:14_

---

_Review comment by @Daverball on `crates/ruff_linter/src/importer/mod.rs`:164 on 2025-02-06 08:14_

This should retain the no-op edit in the most common case (the top-most `TYPE_CHECKING` block is defined in global scope). Although this brought up a question about the implementation of `Importer`:

Why are we adding both the if statement and the body AST node to `type_checking_blocks` in global scope but only the body AST node elsewhere? I don't really see an obvious reason. Shouldn't we just always pass the body or always the entire if statement? Or at least either one or the other, but never both. Either way it is pretty confusing and it would seem more consistent to pass in something like a struct that contains the condition, the body and the scope. That would also make it flexible enough to support a non-idiomatic `if not TYPE_CHECKING` with an `else` clause.

---

_@MichaReiser approved on 2025-02-06 13:45_

This is great. Thank you. Also thanks for reviewing the ecosytem checks so carefully.

---

_Merged by @MichaReiser on 2025-02-06 13:45_

---

_Closed by @MichaReiser on 2025-02-06 13:45_

---

_Branch deleted on 2025-02-06 14:03_

---
