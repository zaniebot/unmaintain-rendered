```yaml
number: 11314
title: "F401 - Recommend adding unused import bindings to `__all__`"
type: pull_request
state: merged
author: plredmond
labels: []
assignees: []
merged: true
base: main
head: ruff.__all__
created_at: 2024-05-06T19:16:09Z
updated_at: 2024-05-15T00:02:34Z
url: https://github.com/astral-sh/ruff/pull/11314
synced_at: 2026-01-10T22:05:26Z
```

# F401 - Recommend adding unused import bindings to `__all__`

---

_Pull request opened by @plredmond on 2024-05-06 19:16_

Followup on #11168 and resolve #10391

# User facing changes

* F401 now recommends a fix to add unused import bindings to to `__all__` if a single `__all__` list or tuple is found in `__init__.py`.
  * If there are no `__all__` found in the file, fall back to recommending redundant-aliases.
  * If there are multiple `__all__` or only one but of the wrong type (non list or tuple) then diagnostics are generated without fixes.
* `fix_title` is updated to reflect what the fix/recommendation is.

Subtlety: For a renamed import such as `import foo as bees`, we can generate a fix to add `bees` to `__all__` but cannot generate a fix to produce a redundant import (because that would break uses of the binding `bees`).

# Implementation changes

* Add `name` field to `ImportBinding` to contain the name of the _binding_ we want to add to `__all__` (important for the `import foo as bees` case). It previously only contained the `AnyImport` which can give us information about the import but not the binding.
  * Add `binding` field to `UnusedImport` to contain the same. (Naming note: the field `name` field already existed on `UnusedImport` and contains the qualified name of the imported symbol/module)
* Change `fix_by_reexporting` to branch on the size of `dunder_all: Vec<&Expr>`
  * For length 0 call the edit-producing function `make_redundant_alias`.
  * For length 1 call edit-producing function `add_to_dunder_all`.
  * Otherwise, produce no fix.
* Implement the edit-producing function `add_to_dunder_all` and add unit tests.
* Implement several fixture tests: empty `__all__ = []`, nonempty `__all__ = ["foo"]`, mis-typed `__all__ = None`, plus-eq `__all__ += ["foo"]`
* `UnusedImportContext::Init` variant now has two fields: whether the fix is in `__init__.py` and how many `__all__` were found.

# Other changes

* Remove a spurious pattern match and instead use field lookups b/c the addition of a field would have required changing the unrelated pattern.
* Tweak input type of `make_redundant_alias`

---

_Comment by @plredmond on 2024-05-06 19:19_

Oops, didn't add clippy to my pre-push hook. Added now.

---

_Comment by @github-actions[bot] on 2024-05-06 19:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @plredmond on 2024-05-06 20:07_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:155 on 2024-05-07 06:00_

What does the `- TextSize::from(1)` stand for? What we do in other places is to explicitly call e.g. `']'.text_len()` to make it clear what kind of character we're subtracting. If there are multiple possible characters, consider adding a comment. 

Is it guaranteed that `__all__` is always a list or set at this point?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:178 on 2024-05-07 06:03_

I think it's also valid to use a set here (CC: @AlexWaygood)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:434 on 2024-05-07 06:08_

Is the `collect` here necessary, considering that you pass the iterator to `make_redundant_alias`?

---

_@MichaReiser reviewed on 2024-05-07 06:08_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:178 on 2024-05-07 06:24_

I don't know about a set, but it's definitely valid (and common) to use a tuple, so we should definitely account for that!

---

_@AlexWaygood reviewed on 2024-05-07 06:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:178 on 2024-05-07 06:36_

ah, it was a tuple! Thanks

---

_@MichaReiser reviewed on 2024-05-07 06:36_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/edits.rs`:162 on 2024-05-07 11:50_

I believe this is going to mean that the added strings will always use double quotes, but that might look odd if all the other strings in the `__all__` sequence use single quotes. Consider using the `ruff_python_codegen::Stylist::quote()` method to determine the standard quoting style used by the source code in question, e.g.

https://github.com/astral-sh/ruff/blob/6774f27f4b96725084b04711aa5aec2c6ac2a597/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L278-L284

You can get a `Stylist` instance by calling `checker.stylist()` in a Ruff rule that has access to a `Checker` instance.

---

_@AlexWaygood reviewed on 2024-05-07 11:55_

---

_@MichaReiser reviewed on 2024-05-07 12:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/edits.rs`:162 on 2024-05-07 12:42_

Nice find!

---

_@plredmond reviewed on 2024-05-07 15:48_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:155 on 2024-05-07 15:48_

Willfix: Comment and use of `text_len`

~~Yes, `dunder_all` is guaranteed to be an `ExprList` by the type of this function~~

---

_@plredmond reviewed on 2024-05-07 15:56_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:434 on 2024-05-07 15:56_

I'd like to avoid the collect if possible. I wasn't able to figure out how to obtain a reference to the result of calling `b.import.member_name()` on each element of the `imports` without collecting an intermediate vector.

Passing `imports.iter().map(|b| b.import.member_name())` directly into `make_redundant_alias` gets:
```
436 |         fix::edits::make_redundant_alias(imports.iter().map(|b| b.import.member_name()), statement)
    |         -------------------------------- ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ expected `&str`, found `Cow<'_, str>`
```
So the closure passed to map must convert the `Cow<'_, str>` to `&str`, but using `.as_ref()` in that closure causes an ownership error:
```
437 |             imports.iter().map(|b| b.import.member_name().as_ref()),
    |                                    ----------------------^^^^^^^^^
    |                                    |
    |                                    returns a value referencing data owned by the current function
    |                                    temporary value created here
```
Suggestions are appreciated

---

_@plredmond reviewed on 2024-05-07 15:59_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:162 on 2024-05-07 15:59_

Great! I was wondering how to do this properly. Willfix

---

_@plredmond reviewed on 2024-05-07 16:31_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:178 on 2024-05-07 16:31_

~~Oops, I found that there is already a data structure for this in `ruff_python_ast`. Will fix.~~ It doesn't contain the necessary information.

---

_@MichaReiser reviewed on 2024-05-07 16:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:434 on 2024-05-07 16:42_

Hmm. Considering that `make_redundant_alias` is only called from `unused_import`. How about you move the implementation and simply make it accept a `Cow`? It doesn't seem worth figuring this puzzle out if we can make our lives easier by just changing the function signature ;)

---

_@plredmond reviewed on 2024-05-07 21:34_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:155 on 2024-05-07 21:34_

The structure of `dunder_all` is now checked in the body of this function

---

_@plredmond reviewed on 2024-05-07 21:34_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:178 on 2024-05-07 21:34_

Ended up using `Expr`

---

_@plredmond reviewed on 2024-05-07 21:34_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:434 on 2024-05-07 21:34_

:shrug: ok!

---

_Comment by @plredmond on 2024-05-07 21:53_

I swear I have fixed my pre-push hook now.

---

_@AlexWaygood reviewed on 2024-05-07 21:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/edits.rs`:158 on 2024-05-07 21:54_

What if it's an unparenthesized tuple 👀 I think this would give incorrect results for something like e.g. https://github.com/python/cpython/blob/460546529b0959ce9528dec1c5cd836dcc04ad2c/Lib/asyncio/base_events.py#L54

There's a field on the `ast::ExprTuple` node that you can query to check whether or not the tuple is parenthesized in the original Python source code 

---

_@plredmond reviewed on 2024-05-07 21:55_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:158 on 2024-05-07 21:55_

Oh what, this is horrible, but ok; willfix

---

_@plredmond reviewed on 2024-05-08 00:19_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:158 on 2024-05-08 00:19_

I think I've resolved this.

---

_Review requested from @AlexWaygood by @plredmond on 2024-05-08 16:35_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_27__all_mistyped____init__.py.snap`:9 on 2024-05-08 17:24_

This message feels kinda confusing to me right now; would it be possible for the message to say something like this instead?

```suggestion
  = help: Use an explicit re-export: `from . import unused as unused`
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 17:24_

What happens if you have something like

```py
import sys

from . import exported, renamed as bees

if sys.version_info > (3, 9):
    from . import also_exported

__all__ = ["exported"]

if sys.version_info >= (3, 9):
    __all__ += ["also_exported"]
```

?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/edits.rs`:174 on 2024-05-08 17:30_

nit: I'd probably do something like

```suggestion
        ast::Expr::Tuple(ast::ExprTuple {
            elts,
            range,
            parenthesized,
        }) => (
            if parenthesized {
                elts.last()
                    .map_or(range.end() - ")".text_len(), |elt| elt.range().end())
            } else {
                elts.last()
                    .expect("unparenthesized empty tuple is not possible")
                    .range()
                    .end()
            },
            elts.len(),
        ),
```

(just personal preference)

---

_@AlexWaygood reviewed on 2024-05-08 17:31_

---

_@plredmond reviewed on 2024-05-08 18:11_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 18:11_

The expected behavior would be for the fix will add `"bees"` to `__all__ = ["exported"]` but i'll add a fixture test for this

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 18:12_

If it's adding to the last one, that's a mistake; but now that I think about it, that's what the existing fixture test does .. Hmm

---

_@plredmond reviewed on 2024-05-08 18:12_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:174 on 2024-05-08 18:13_

I'm going to use two separate cases instead of nested conditionals here :)

---

_@plredmond reviewed on 2024-05-08 18:13_

---

_@AlexWaygood reviewed on 2024-05-08 18:31_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 18:31_

See I'm wondering if we should just not attempt to do an autofix at all if there's more than one `__all__` definition at the top level, since this kind of thing can get arbitrarily complicated

---

_@AlexWaygood reviewed on 2024-05-08 18:33_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 18:33_

E.g. checkout https://github.com/python/typeshed/blob/2d33fe212221a05661c0db5215a91cf3d7b7f072/stdlib/typing.pyi#L106

---

_@plredmond reviewed on 2024-05-08 18:54_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 18:54_

Changed to add to the first `__all__` binding.. this is of course still vulnerable to e.g.
```python
if foo.version < 3:
    __all__ = ["foo"]
else:
    __all__ = []
```
Here I believe we'd only add to the first `__all__` and not the 2nd. I think we could play whack-a-mole with these cases for awhile. What should we do?

---

_@AlexWaygood reviewed on 2024-05-08 18:58_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-08 18:58_

This is better but I'd still find this message confusing if it popped up in my editor, especially if I wasn't familiar with the (slightly odd) convention that static-analysis tools use to distinguish explicit re-exports from implicit re-exports in modules without `__all__`. If I saw this message, I think my natural interpretation would be to assume it was telling me to do this:

```diff
- from . import unused
+ from . import .unused
```

But that's not what we're telling the user to do at all (it's invalid syntax), and it's not what the autofix does. Could the help message be this instead?

```suggestion
   = help: Use an explicit re-export: `from . import unused as unused`
```

---

_@AlexWaygood reviewed on 2024-05-08 19:01_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 19:01_

https://github.com/astral-sh/ruff/pull/11314#discussion_r1594472417 — I think we should just not attempt to provide an autofix if there's more than one `__all__` definition at the top level

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-08 20:04_

It might take a bit of effort to run the fix manually and generate a more explicit fix title message there. Also, it seems redundant because directly below your comment on the fixture test output is a diff/fix that is doing the thing described. Alternatively, could I suggest that the message could be changed to **not** say the `qualified_name` and instead say the name of the binding? I.e. In this case for `from . import unused` the fix title text would become:
```
help: Use an explicit re-export: `unused`
```
As in the case for `from . import renamed as bees` where the text is already:
```
help: Use an explicit re-export: `bees`
```

---

_@plredmond reviewed on 2024-05-08 20:04_

---

_@plredmond reviewed on 2024-05-08 20:05_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 20:05_

Weird, github didn't show me your comments. I have to shift+f5 github or it doesn't work properly these days. Yeah, I can change it to offer no fix if there's more than one top level `__all__`.

---

_@plredmond reviewed on 2024-05-08 20:13_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 20:13_

Would it be ok to fall-back to recommending a fix that does an explicit re-export?

---

_@plredmond reviewed on 2024-05-08 20:28_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-08 20:28_

Changed to not offer any fix related to `__all__` when there's more than one top level `__all__` binding. Instead, fall back to the redundant-alias path (recommend a fix when the unused import is not renamed).

---

_@AlexWaygood reviewed on 2024-05-09 11:18_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-09 11:18_

> Would it be ok to fall-back to recommending a fix that does an explicit re-export?

Yes, I think that's a much safer fix because we don't have to track the "parallel branches" in the same way as we do with the `__all__` fix. Good call!

---

_@AlexWaygood reviewed on 2024-05-09 11:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-09 11:53_

> it seems redundant because directly below your comment on the fixture test output is a diff/fix that is doing the thing described. 

My worry is that VSCode users, for example, will have this "help" message displayed to them in a message when they hover their mouse over the error in their editor and then click the "Quick fix" button. For example, look at the message next to the lightbulb here:

![image](https://github.com/astral-sh/ruff/assets/66076021/03cd4882-7ac2-4df6-ae73-209b8242b3a9)

The "quick fix" summary message users will see in their editor doesn't show the diff that the autofix would make. It's important that the message provides a succinct and accurate summary of the change the autofix is going to make to the user's code if they double click on that message to apply the "quick fix" in their editor.

---

_Review comment by @zanieb on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-09 13:16_

It seems weird to offer a redundant alias export when there is an `__all__` in the file? I would not be happy with that fix as a user, I'd need to change it manually.

---

_@zanieb reviewed on 2024-05-09 13:16_

---

_@AlexWaygood reviewed on 2024-05-09 13:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-09 13:20_

> It seems weird to offer a redundant alias export when there is an `__all__` in the file? I would not be happy with that fix as a user, I'd need to change it manually.

Yeah that's a good point. Maybe better to just not offer a fix at all if there are multiple `__all__` definitions, then

---

_@AlexWaygood reviewed on 2024-05-09 14:32_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:17 on 2024-05-09 14:32_

I'm not sure there's much value in a fixture like this, as it doesn't seem particularly realistic to me. In real code, you'd only have multiple `__all__` definitions at the top level if there was some kind of version or platform difference that made an attribute sometimes available, sometimes not (and as soon as you have that kind of branching, attempting an autofix becomes much more complex, as we've discussed!). So I think the example you added in `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_30__all_conditional/__init__.py` is much better; I'd remove this example, I think.

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-09 15:44_

```
0 __all__ definitions -> convert unused imports to redundant aliases w/fix
1 __all__ definitions -> add unused imports to __all__ w/fix
otherwise -> diagnostic w/o fix

---

_@plredmond reviewed on 2024-05-09 15:44_

---

_@plredmond reviewed on 2024-05-09 15:48_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:17 on 2024-05-09 15:48_

Ok, I'll remove it

---

_@plredmond reviewed on 2024-05-09 16:18_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-09 16:18_

I've made this update. Now I'm wondering if there's a desired change to [this fixture test](https://github.com/astral-sh/ruff/pull/11314/files#diff-a444752923fd8532aa73acfb55601c2ae52153ea4bf396f20d2b8ee088e7faa4) in the case where `__all__` is neither tuple nor list. The current implementation offers a fix to use a redundant alias in that case. Should I change it to conservatively offer no fix and recommend removal, as with the multiple-`__all__` case?

---

_@plredmond reviewed on 2024-05-09 16:20_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-09 16:20_

Is `use an explicit re-export: 'bees'` inaccurate or not succinct enough? What is different about this fix on this rule that it requires a diff in its fix title?

---

_Comment by @plredmond on 2024-05-09 16:25_

Almost there, maybe? @zanieb @AlexWaygood  Two unresolved threads left.

---

_@AlexWaygood reviewed on 2024-05-09 16:30_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-09 16:30_

I don't think the message is clear enough about the change that it's going to make if you double-click on that message in your editor. If you're a Ruff user and you're not familiar with this (slightly weird) convention using redundant aliases that static-analysis tools use to differentiate explicit re-exports from implicit re-exports in modules without `__all__`, you won't know what's meant by the term "explicit re-export". ("Redundant alias" is a more descriptive term for the concept, but even more confusing -- why is Ruff suggesting to me to add an alias if it admits that doing so would be redundant?)

So I would prefer that the help message explicitly states what it's going to change your code into if you double-click on it, similar to the SIM108 help message in the screenshot above.

---

_@plredmond reviewed on 2024-05-09 16:33_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-09 16:33_

Ok, there's a bit of code required to produce an actual diff. I wrote some of that over [in this test](https://github.com/astral-sh/ruff/pull/11314/files#diff-c93288320b9364fb125a43ff42ff5dc9780a16989eb5f0d1ecf903cd6d9a281cR681-R690). Would be ok to package something like that up into a helper function in the unused_imports.rs file, and use it to generate the fix-title?

---

_@AlexWaygood reviewed on 2024-05-09 16:38_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-09 16:38_

If creating the diff is hard, I'd be okay with using pseudo-code that doesn't reference the actual symbols in the user's code, e.g.

```suggestion
   = help: Use an explicit re-export (`from module import thing as thing`)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-09 16:39_

Whatever the case, `.unused` is especially confusing, since `from . import .unused` is invalid syntax, and I feel like that's the natural reading of what the help message is proposing to do to the code :-)

---

_@AlexWaygood reviewed on 2024-05-09 16:39_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-09 16:48_

> I've made this update. Now I'm wondering if there's a desired change to [this fixture test](https://github.com/astral-sh/ruff/pull/11314/files#diff-a444752923fd8532aa73acfb55601c2ae52153ea4bf396f20d2b8ee088e7faa4) in the case where `__all__` is neither tuple nor list. The current implementation offers a fix to use a redundant alias in that case. Should I change it to conservatively offer no fix and recommend removal, as with the multiple-`__all__` case?

Yeah, offering no fix there makes sense to me there as well...

---

_@AlexWaygood reviewed on 2024-05-09 16:48_

---

_@plredmond reviewed on 2024-05-09 17:01_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-09 17:01_

The use of the qualified name in all the fix messages predates my work on F401. If I change that, we'll have ~25 additional snapshot tests changed in this PR. Before I make that change (to use the binding name, instead of the module qualified name, in fix titles), is that acceptable?

---

_@plredmond reviewed on 2024-05-09 17:07_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_29__all_pluseq/__init__.py`:15 on 2024-05-09 17:07_

Great. Made this change.

---

_@plredmond reviewed on 2024-05-09 17:08_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-09 17:08_

I'll try making a pseudocode fix-title and let's see how that looks.

---

_Comment by @plredmond on 2024-05-09 17:37_

I believe that i've resolved the outstanding requests. @AlexWaygood it might make sense to do a pass over the fixture tests now since there has been some churn, to see if you agree with how each case is resolved?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_5.py.snap`:30 on 2024-05-10 09:23_

The change in this PR is a good improvement, so this comment shouldn't block this PR being merged, _but_... I think what would be even better would be if it said:

```suggestion
  = help: Remove unused import: `f as g`
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_24____init__.py.snap`:25 on 2024-05-10 09:24_

This is so much clearer imo -- thank you for fixing this!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_25__all_nonempty____init__.py.snap`:25 on 2024-05-10 09:27_

Hmm, not sure about this one. The help message sounds like it's literally going to add the string `".unused"` to `__all__` -- but it's actually going to add the string `"unused"` to `__all__`. I think one of the following two options would be much better:

```suggestion
__init__.py:36:15: F401 [*] `.unused` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
   |
36 | from . import unused # F401: add to __all__
   |               ^^^^^^ F401
   |
   = help: Add `unused` to `__all__`
```

or 

```suggestion
__init__.py:36:15: F401 [*] `.unused` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
   |
36 | from . import unused # F401: add to __all__
   |               ^^^^^^ F401
   |
   = help: Add `"unused"` to `__all__`
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/edits.rs`:193 on 2024-05-10 09:32_

```suggestion
    if let ast::Expr::Tuple(tup) = expr {
        if tup.parenthesized && export_prefix_length + edits.len() == 1 {
            edits.push(Edit::insertion(",".to_string(), insertion_point));
        }
    }
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:176 on 2024-05-10 09:44_

```suggestion
            if !binding.kind.is_export() {
                return None;
            }
            match binding.statement(semantic)? {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/edits.rs`:620 on 2024-05-10 09:50_

nit: using `::from` in cases like this is usually more readable than using `std::convert::Into::into`

```suggestion
            make_redundant_alias(["x"].into_iter().map(Cow::from), stmt),
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:446 on 2024-05-10 09:52_

```suggestion
        _ => bail!("Cannot offer a fix when there are multiple __all__ definitions"),
```

---

_@AlexWaygood reviewed on 2024-05-10 09:54_

Much improved, thanks for your patience! A few more comments below.

---

_@plredmond reviewed on 2024-05-13 17:59_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:176 on 2024-05-13 17:59_

I think it's clearer to downcast from binding to statement in one statement here

---

_@plredmond reviewed on 2024-05-13 18:00_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:193 on 2024-05-13 18:00_

The suggested change splits up a single condition into two separate conditions, which I think is less clear. However, it removes an empty match branch which ppl might not like, so I've committed it.

---

_@plredmond reviewed on 2024-05-13 18:01_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:620 on 2024-05-13 18:01_

Thank you

---

_@plredmond reviewed on 2024-05-13 18:10_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_25__all_nonempty____init__.py.snap`:25 on 2024-05-13 18:10_

I've gone with the following:
```
   21       │-   = help: Add unused import to __all__: `.unused`
         21 │+   = help: Add unused import `unused` to __all__
```
```
   35       │-   = help: Add unused import to __all__: `bees`
         35 │+   = help: Add unused import `bees` to __all__
```
imo, it is important to say /why/ it should be added to `__all__`. I opted for the binding instead of the quoted-binding because I think it reads easier.

---

_@AlexWaygood reviewed on 2024-05-13 18:12_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_25__all_nonempty____init__.py.snap`:25 on 2024-05-13 18:12_

SGTM

---

_@plredmond reviewed on 2024-05-13 18:38_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_5.py.snap`:30 on 2024-05-13 18:38_

I think I can make the code do this by by replacing the `UnusedImport.name:String` field with `UnusedImport.import:&AnyImport<'_>`. This frees up the fix-title generating code to choose from more options than just: fully-qualified module path or alias/binding. I'll spend a few minutes to see how hard this will be.

---

_@plredmond reviewed on 2024-05-13 18:48_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__F401_F401_5.py.snap`:30 on 2024-05-13 18:48_

My suggestion above won't work due to the lifetimes involved. It could be implemented by including the appropriate string (for `f`) as an additional field on `UnusedImport`. Let's consider it for a future PR.

---

_Review requested from @zanieb by @plredmond on 2024-05-13 18:57_

---

_Comment by @plredmond on 2024-05-13 18:58_

This is ready for review again.

---

_@zanieb approved on 2024-05-14 14:33_

Awesome, this is a huge improvement to this rule 🎉 

---

_@charliermarsh reviewed on 2024-05-14 14:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/fix/edits.rs`:164 on 2024-05-14 14:44_

I believe `tup.range.end()` can just be `tup.end()`, and `elt.range().end()` can just be `elt.end()`.

---

_@charliermarsh reviewed on 2024-05-14 14:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/fix/edits.rs`:161 on 2024-05-14 14:44_

By the way, I think the `ast` prefixes here are unnecessary:

![Screenshot 2024-05-14 at 10 44 17 AM](https://github.com/astral-sh/ruff/assets/1309177/37c926c3-9e2d-4257-9558-5e9f917db3fa)


---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_26__all_empty/__init__.py`:12 on 2024-05-14 14:46_

Can you remove the trailing newlines here for consistency with the other fixtures? Otherwise, editors that automatically strip trailing newlines will lead to unnecessary edits here in the future. (You can run `ruff format` over these if it's easier.)


---

_@charliermarsh reviewed on 2024-05-14 14:46_

---

_Comment by @charliermarsh on 2024-05-14 14:47_

(A few small comments but for clarity no need to wait on my approval to merge.)

---

_@plredmond reviewed on 2024-05-14 17:24_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:164 on 2024-05-14 17:24_

Thanks

---

_@plredmond reviewed on 2024-05-14 17:26_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:161 on 2024-05-14 17:26_

Oops, fixed

---

_@plredmond reviewed on 2024-05-14 17:34_

---

_Review comment by @plredmond on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_26__all_empty/__init__.py`:12 on 2024-05-14 17:34_

Yes, sorry, I forgot to `ruff format` them

---

_Comment by @codspeed-hq[bot] on 2024-05-14 17:35_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ruff.__all__)

### Merging #11314 will **degrade performances by 6.83%**

<sub>Comparing <code>ruff.__all__</code> (dde9756) with <code>main</code> (96f6288)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ruff.__all__)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ruff.__all__` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 7.5 ms | 8 ms | -6.83% |


---

_Comment by @charliermarsh on 2024-05-14 17:37_

I suspect the CodSpeed regression is a false positive.

---

_Comment by @plredmond on 2024-05-14 17:38_

Maybe it'll re-run after this last pull and resolve (if codespeed updates its comments).

---

_Comment by @AlexWaygood on 2024-05-14 17:39_

Rebasing or merging in `main` often gets CodSpeed to change its mind as well

---

_Comment by @plredmond on 2024-05-14 17:40_

Ah, that's a good point. If we're comparing to `main` the performance could very well be different on an outdated branch.

---

_Comment by @plredmond on 2024-05-14 17:43_

Rebasing.

---

_Comment by @plredmond on 2024-05-14 17:56_

I've updated the PR description to reflect the diff after all review comments.

Not sure what to do next. CodeSpeed might be a false-positive, but that should have been mitigated by rebasing.

---

_@charliermarsh reviewed on 2024-05-14 18:00_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/fix/edits.rs`:669 on 2024-05-14 18:00_

What does `SUT` mean?

---

_Comment by @charliermarsh on 2024-05-14 18:01_

I think it's ok to merge personally.

---

_@plredmond reviewed on 2024-05-14 18:01_

---

_Review comment by @plredmond on `crates/ruff_linter/src/fix/edits.rs`:669 on 2024-05-14 18:01_

system under test -- the test is long so i'm marking the bit that's unit tested

---

_Comment by @charliermarsh on 2024-05-14 18:01_

I'm looking through the changes again just to see if anything sticks out.

---

_@AlexWaygood reviewed on 2024-05-14 18:02_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/fix/edits.rs`:669 on 2024-05-14 18:02_

This acronym was also unfamiliar to me FWIW

---

_Comment by @charliermarsh on 2024-05-14 18:02_

The only thing that stands out is that we're allocating an additional string for `UnusedImport` and we increased the size of the struct.

---

_Comment by @charliermarsh on 2024-05-14 18:06_

We could consider using `binding` everywhere for the diagnostic instead of the qualified name, see if that helps? But I think it's fine to merge as-is.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:182 on 2024-05-14 18:08_

Possible optimisation here: if there's more than one `__all__` definition, we won't do an autofix anyway, so we only need to collect a maximum of two `__all__` definitions into the `Vec` being returned

```suggestion
        })
        .take(2)
```

---

_@AlexWaygood reviewed on 2024-05-14 18:08_

---

_@charliermarsh reviewed on 2024-05-14 18:13_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:182 on 2024-05-14 18:13_

I'd probably vote to leave it as-is -- it makes sense (we can avoid some binding lookups), but seems unlikely to matter and creates some implicit coupling between this method and the fix rules that we may forget about in the future.

---

_@plredmond reviewed on 2024-05-14 18:23_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:182 on 2024-05-14 18:23_

Whoops. I clicked the button. I'll revert if it's not conclusive.

---

_@charliermarsh reviewed on 2024-05-14 18:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:182 on 2024-05-14 18:24_

I defer to you as the author, either is fine :)

---

_@plredmond reviewed on 2024-05-14 18:26_

---

_Review comment by @plredmond on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:182 on 2024-05-14 18:26_

Interesting. No effect. :)) I'll revert.

---

_@AlexWaygood reviewed on 2024-05-14 18:27_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs`:182 on 2024-05-14 18:27_

Yeah, no difference at all :( Ah, well.

---

_Comment by @plredmond on 2024-05-14 18:35_

Trying out Charlie's suggestion of using only one string in the violation struct. This caused /a lot/ of fixture changes to have cosmetic changes to help text and rule titles. If it doesn't correct the regression I'll revert.

---

_Comment by @plredmond on 2024-05-14 20:52_

Ok, that didn't work so I'm going to revert and then merge once tests pass.

---

_Comment by @plredmond on 2024-05-14 21:47_

Ok! I've tried (and reverted) both of the suggested performance improvements. Do we want to just merge this? I have no more commits to push.

---

_Comment by @charliermarsh on 2024-05-14 22:56_

Yeah, I'm fine with going ahead and merging.

---

_Merged by @plredmond on 2024-05-15 00:02_

---

_Closed by @plredmond on 2024-05-15 00:02_

---

_Branch deleted on 2024-05-15 00:02_

---
