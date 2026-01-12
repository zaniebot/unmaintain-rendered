```yaml
number: 15862
title: "[`pyupgrade`] Rename private type parameters in PEP 695 generics (`UP049`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/rename-typevars
created_at: 2025-01-31T22:04:48Z
updated_at: 2025-02-04T18:37:04Z
url: https://github.com/astral-sh/ruff/pull/15862
synced_at: 2026-01-12T15:55:52Z
```

# [`pyupgrade`] Rename private type parameters in PEP 695 generics (`UP049`)

---

_@ntBre_

## Summary

This is a new rule to implement the renaming of PEP 695 type parameters with leading underscores after they have (presumably) been converted from standalone type variables by either UP046 or UP047. Part of #15642.

I'm not 100% sure the fix is always safe, but I haven't come up with any counterexamples yet. `Renamer` seems pretty precise, so I don't think the usual issues with comments apply.

I initially tried writing this as a rule that receives a `Stmt` rather than a `Binding`, but in that case the `checker.semantic().current_scope()` was the global scope, rather than the scope of the type parameters as I needed. Most of the other rules using `Renamer` also used `Binding`s, but it does have the downside of offering separate diagnostics for each parameter to rename.

## Test Plan

New snapshot tests for UP049 alone and the combination of UP046, UP049, and PYI018.


---

_Label `rule` added by @ntBre on 2025-01-31 22:04_

---

_Label `preview` added by @ntBre on 2025-01-31 22:04_

---

_Comment by @github-actions[bot] on 2025-01-31 22:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@InSyncWithFoo reviewed on 2025-01-31 22:19_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:47 on 2025-01-31 22:19_

These `.md` links won't work when rendered in an editor. Typically, links between rules take the full form `https://docs.astral.sh/ruff/rules/rule-name/`. Examples include [`D211`](https://github.com/astral-sh/ruff/blob/b58f2c399e87c11def59bd55f75f7ef8148c84f0/crates/ruff_linter/src/rules/pydocstyle/rules/blank_before_after_class.rs#L123) and [`S606`](https://github.com/astral-sh/ruff/blob/b58f2c399e87c11def59bd55f75f7ef8148c84f0/crates/ruff_linter/src/rules/flake8_bandit/rules/shell_injection.rs#L197).

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051.py`:20 on 2025-01-31 22:26_

Are references within stringified type expressions renamed?

```python
def f[_T](v):
	cast('_T', v)             # This should be renamed
	cast("Literal['_T']", v)  # This should not
	cast('list[_\x54]', v)    # Weird, but...
```

---

_@InSyncWithFoo reviewed on 2025-01-31 22:26_

---

_@ntBre reviewed on 2025-01-31 22:33_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051.py`:20 on 2025-01-31 22:33_

No the `Renamer` does not catch these, I guess that's enough to make the fix unsafe.

---

_@ntBre reviewed on 2025-01-31 22:36_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051.py`:20 on 2025-01-31 22:36_

Never mind, they *both* get renamed when `cast` is actually imported. Why should the second one not get renamed? Is it supposed to be outside of the function? I "fixed" the indentation by putting them both inside.

---

_@InSyncWithFoo reviewed on 2025-01-31 22:39_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051.py`:20 on 2025-01-31 22:39_

The second one, when unquoted, is `Literal['_T']` (i.e., a string containing the two characters `_` and `T`). It does not refer to `_T` the type parameter.

---

_@ntBre reviewed on 2025-01-31 22:42_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051.py`:20 on 2025-01-31 22:42_

Ah right, thanks. I guess I spoke too soon again. If `Literal` is also imported, I think we get the correct behavior for your original example (the first one replaced, the second not). I will throw the `\x54` case in as well just for fun. Thanks for these additional cases!

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051.py`:20 on 2025-01-31 22:47_

Well this isn't good. `cast('list[_\x54]', v) ` gets replaced with `cast(T, v)`, while `cast("list[_T]", v)` gets replaced correctly with `cast("list[T]", v)`. I'm not sure why the hex version is deleting the `list[]` part. Is this something you've run into before?

---

_@ntBre reviewed on 2025-01-31 22:47_

---

_@InSyncWithFoo reviewed on 2025-01-31 22:58_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051.py`:20 on 2025-01-31 22:58_

I don't think so. I wonder if such cases can be detected somehow so that the fix can be marked unsafe/display-only. Another approach is to use different fix applicability for stub and source files; fixes for the former are most likely safe, since it should have no stringified type expressions and very few `cast()`s.

---

_@AlexWaygood reviewed on 2025-02-03 13:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP051_1.py.snap`:1 on 2025-02-03 13:11_

I'm not exactly sure why the UP051 errors aren't being shown in this snapshot. I don't think this indicates a bug in the logic for your rule, though, as everything seems to work correctly if I run the rules manually together on this fixture with your branch:

````
~/dev/ruff (brent/rename-typevars) [1] % cargo run -- check --no-cache --preview --diff --select="PYI018,UP051,UP046,F401" --target-version=py313 --unsafe-fixes crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051_1.py
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff check --no-cache --preview --diff --select=PYI018,UP051,UP046,F401 --target-version=py313 --unsafe-fixes crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051_1.py`
--- crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051_1.py
+++ crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051_1.py
@@ -1,4 +1,4 @@
-from typing import Generic, TypeVar
+from typing import TypeVar
 
 # this case should not be affected by UP051 alone, but with UP046 and PYI018,
 # the old-style generics should be replaced with type parameters, the private
@@ -7,5 +7,5 @@
 _T = TypeVar("_T")
 
 
-class OldStyle(Generic[_T]):
-    var: _T
+class OldStyle[T]:
+    var: T

Would fix 3 errors.
````

The only thing that surprises me there is that the PYI018 violation introduced by the UP046 change introduces is not autofixed. Apparently we don't have an autofix for PYI018 right now; I suppose I assumed that we did!

My guess is that the `assert_messages!()` macro we use in our tests maybe doesn't run Ruff in a loop in the same way as `ruff check` does when it's run from the command line? Not sure. @MichaReiser, any ideas?

One solution might be to write this as a CLI integration test, if our standard testing infrastructure isn't up to the task here.

---

_@AlexWaygood reviewed on 2025-02-03 13:17_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051.py`:20 on 2025-02-03 13:17_

> Well this isn't good. `cast('list[_\x54]', v) ` gets replaced with `cast(T, v)`, while `cast("list[_T]", v)` gets replaced correctly with `cast("list[T]", v)`. I'm not sure why the hex version is deleting the `list[]` part. Is this something you've run into before?

This sounds like it could be a bug in the `Renamer`. It also seems like an extreme edge case, however, that's unlikely to actually come up in either handwritten or generated code -- so personally I think the fix could still be marked as safe.

---

_@AlexWaygood reviewed on 2025-02-03 13:18_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:71 on 2025-02-03 13:18_

```suggestion
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
enum ParamKind {
```

---

_@AlexWaygood reviewed on 2025-02-03 13:20_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-03 13:20_

I came across this the other day -- unfortunately I think `binding.name()` does the wrong thing here for PEP-695 type parameters with bounds or constraints. E.g. for `_T` here:

```py
class Foo[_T: str]: ...
```

I believe `binding.name()` will return `"_T: str"` rather than `"_T"`

---

_@AlexWaygood reviewed on 2025-02-03 13:21_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-03 13:21_

This sort of feels like a footgun in our `Binding` API that I don't really like; we should possibly try to fix that

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:100 on 2025-02-03 13:33_

What happens here if we have a PEP-695 variable named `_` or `__`?

I think this will also flag e.g. `class Foo[_T_]: ...` and `class Foo[__T__]: ...` ("sunder" and "dunder" variables, which are arguably not "private"). Do we want to skip the rule if the name also ends in `_`?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:136 on 2025-02-03 13:37_

we probably want to check various thinsg before doing the rename, such as:
- whether the new name would be a keyword (changing `class F[_async]: ...` to `class F[async]: ...` would introduce invalid syntax)
- whether the new name would shadow an existing variable in an outer scope

A lot of this logic is already done in this function for RUF052:

https://github.com/astral-sh/ruff/blob/83243de93de6b63ab6de3a6b77113416c3eb51d9/crates/ruff_linter/src/rules/ruff/rules/used_dummy_variable.rs#L239-L271

We could possibly move that function to `renamer.rs` if it would also be helpful here

---

_@AlexWaygood reviewed on 2025-02-03 13:38_

Overall this looks great!

---

_@ntBre reviewed on 2025-02-03 20:15_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:47 on 2025-02-03 20:15_

Thanks, and I see you already updated the related rules in https://github.com/astral-sh/ruff/pull/15904.

---

_@ntBre reviewed on 2025-02-03 20:18_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051.py`:20 on 2025-02-03 20:18_

Sounds good. For now I've included the other three cases and left the rule as safe. I'll try to add a `Renamer` test for the hex case and debug that separately.

---

_@ntBre reviewed on 2025-02-03 20:27_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-03 20:27_

Ah good catch, that's what I get for only testing the simplest cases. Somewhat fortunately this causes an `Err` return from the `Renamer`, so we get a diagnostic without a fix (or would if I hadn't marked the fix as always available). But I'll try to do better than that still.

---

_@ntBre reviewed on 2025-02-03 20:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:100 on 2025-02-03 20:29_

Ah great catches again. I think it makes sense to skip variables that also end in `_` and obviously if the new name is empty :facepalm:

---

_@ntBre reviewed on 2025-02-03 20:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-03 20:35_

This also completely skips `TypeVarTuple`s and `ParamSpec`s because they "start with" `*` and `**`.

---

_@ntBre reviewed on 2025-02-03 21:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-03 21:22_

I think I will have to do some work on either `Binding` or `Renamer`. I tried looping over the `type_params` directly to get just the `_T` part of the name, but `Renamer` finds the `Binding` for the name I'm giving it and replaces the whole `Binding::range` anyway.

---

_@ntBre reviewed on 2025-02-03 22:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-03 22:02_

I think I have the `Binding` range fix. I was considering a separate PR, but it's a one-line change in the semantic model and two one-line changes in PYI019, so I think I'll just include it here. This rule would have to be reverted with it anyway.

---

_@ntBre reviewed on 2025-02-03 23:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:136 on 2025-02-03 23:20_

Nice, thanks for this suggestion! It's kind of a shame to do the `name.starts_with` and `trim_start_matches` calls twice in both the new rule here and in `used_dummy_variable`. I might try to tweak the API a bit, but the current version passes new tests I added based on your comment!

Do you think we should offer a diagnostic and not a fix here, or bail out entirely? I'm currently bailing out, but it seems fairly reasonable to have a diagnostic, at least without thinking too hard.

---

_@ntBre reviewed on 2025-02-04 02:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP051_1.py.snap`:1 on 2025-02-04 02:22_

I poked around a bit more and from what I can tell, `test_contents` returns the first [`diagnostics`](https://github.com/astral-sh/ruff/blob/brent%2Frename-typevars/crates/ruff_linter/src/test.rs#L123) assignment, even though it does run the fixes in a loop and see the [other](https://github.com/astral-sh/ruff/blob/brent%2Frename-typevars/crates/ruff_linter/src/test.rs#L188) diagnostics. I went ahead and converted to an integration test for now!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-04 11:19_

> I was considering a separate PR, but it's a one-line change in the semantic model and two one-line changes in PYI019, so I think I'll just include it here.

Hmm, I think I _would_ prefer this as a separate PR for a couple of reasons:
1. I think we can actually simplify the code in PYI019 further with this fix
2. Changing the range of `Binding` makes me a _little_ nervous that we might actually change the range of some diagnostics, which would be a breaking change. I'm _pretty_ confident from the ecosystem report and the fact that no snapshots in other rules need to change that we're not changing any diagnostic ranges... but I'm not _100%_ confident. If we move it to an isolated change, it will be easy to bisect it (if necessary) and see exactly what caused the breaking change.

I know exactly what I'd like to do for point (1) here, so I'll file a new PR and list you as a co-author -- hope that's okay!

---

_@AlexWaygood reviewed on 2025-02-04 11:19_

---

_@ntBre reviewed on 2025-02-04 13:26_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-04 13:26_

That makes sense, it definitely makes me nervous too! I was surprised only one test changed as a result.

Sounds good about the the separate PR!

---

_@AlexWaygood reviewed on 2025-02-04 14:03_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-04 14:03_

- https://github.com/astral-sh/ruff/pull/15935

Sorry for the delay, got distracted by things I thought would take a little less time than they did!

---

_@AlexWaygood reviewed on 2025-02-04 14:06_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:136 on 2025-02-04 14:06_

I think having a diagnostic (but not offering a fix) makes sense!

RUF052 also does some stuff to try to find other names it could use instead if the name-sans-leading-underscore is shadowed/keyword/not-an-identifier. I don't think it makes sense to do the same here, though; I think we should just have a diagnostic-without-fix if it's shadowed/keyword/not-an-identifier

---

_@ntBre reviewed on 2025-02-04 14:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-04 14:24_

No worries at all! I'll merge main again.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:136 on 2025-02-04 14:26_

I agree, I'll update this and add a test showing just a diagnostic.

Are we safe to handle `default` here? With the `Binding` change we're just replacing the name, so I was thinking about adding a test with a `default` type parameter too.

---

_@ntBre reviewed on 2025-02-04 14:26_

---

_@AlexWaygood reviewed on 2025-02-04 14:34_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:136 on 2025-02-04 14:34_

> Are we safe to handle `default` here? With the `Binding` change we're just replacing the name, so I was thinking about adding a test with a `default` type parameter too.

Yeah, I don't think TypeVars with defaults need any special treatment here! We should be good to handle those in this rule 👍

---

_@AlexWaygood reviewed on 2025-02-04 14:35_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-04 14:35_

...and also this one 😆

- https://github.com/astral-sh/ruff/pull/15938

---

_@ntBre reviewed on 2025-02-04 14:38_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:114 on 2025-02-04 14:38_

Ahh I thought I had to add `.name()` in two places! Good catch!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP051_1.py.snap`:1 on 2025-02-04 14:52_

Should we open an issue to add a PYI018 autofix? It might be a good `help-wanted` or `first-issue`, unless I'm underestimating it of course.

---

_@ntBre reviewed on 2025-02-04 14:52_

---

_@AlexWaygood reviewed on 2025-02-04 14:53_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP051_1.py.snap`:1 on 2025-02-04 14:53_

that would be great, yeah. I think it would be very useful

---

_Review comment by @AlexWaygood on `crates/ruff/tests/lint.rs`:2186 on 2025-02-04 15:14_

Is it worth throwing F401 into the mix as well, so that the import of `Generic` is removed once it becomes unsued?

---

_Review comment by @AlexWaygood on `crates/ruff/tests/snapshots/lint__pep695_generic_rename.snap`:1 on 2025-02-04 15:15_

nit: I find it much easier to read these integration tests if we use inline snapshots, like this test does:

https://github.com/astral-sh/ruff/blob/5bf0e2e95ed4c9ef3e063c6e3876c59d827cb9cf/crates/ruff/tests/lint.rs#L22-L63

(`cargo insta` can still update the snapshot contents interactively, just like with separate-file snapshots)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/renamer.rs`:416 on 2025-02-04 15:17_

This feels too specific to RUF052 and UP051 to be included in the generic utility in this module (not all potential users of the `Renamer` will be renaming names that start with underscores). I think we should be checking whether it starts with an underscore (and trimming leading underscores) in RUF052/UP051, and then passing the trimmed name into `try_shadowed_kind()`. This would also allow `try_shadowed_kind()` to return `ShadowedKind` rather than `Option<ShadowedKind>`.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:46 on 2025-02-04 15:20_

```suggestion
/// If the name without an underscore would shadow a builtin or another variable,
/// would be a keyword, or would otherwise be an invalid identifier, a fix will not
/// be available. In these situations, you can consider using a trailing underscore
/// or a different name entirely to satisfy the lint rule.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:59 on 2025-02-04 15:21_

I can never remember whether mkdocs will create links correctly if backticks are included in the `[]` names for the footnotes...

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:107 on 2025-02-04 15:23_

nit: I'd find this a bit more readable; these feel like different conditions

```suggestion
    if !old_name.starts_with('_') {
        return None;
    }
     
    // Sunder `_T_`, dunder `__T__`, and all all-under `_` or `__` cases
    // should all be skipped, as these are not "private" names
    if old_name.ends_with('_') {
        return None;
    }
```

It would also be good to mention this logic explicitly in the docs for the rule!

---

_@AlexWaygood reviewed on 2025-02-04 15:23_

---

_@AlexWaygood reviewed on 2025-02-04 15:24_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/renamer.rs`:391 on 2025-02-04 15:24_

```suggestion
    pub(crate) const fn shadows_any(self) -> bool {
```

---

_@AlexWaygood reviewed on 2025-02-04 15:25_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:79 on 2025-02-04 15:25_

```suggestion
}

impl ParamKind {
    const fn as_str(self) -> &'static str {
        match self {
            Self::Class => "class",
            Self::Function => "function",
        }
    }
}

impl Violation for PrivateTypeParameter {
    const FIX_AVAILABILITY: FixAvailability = FixAvailability::Sometimes;
    #[derive_message_formats]
    fn message(&self) -> String {
        format!("Generic {} uses private type parameters", self.kind.as_str())
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051_1.py`:49 on 2025-02-04 15:28_

it might be more realistic if `X` is an actual type that it would be valid to annotate `x` with?

```suggestion
    type X = int

    class ScopeConflict[_X]:
        var: _X
        x: X
```

---

_@AlexWaygood reviewed on 2025-02-04 15:28_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP051_1.py`:1 on 2025-02-04 15:29_

Could you possibly add these tests, just for completeness? I think none of them should cause us to emit diagnostics:

```py
def f[_](x: _) -> _: ...
def g[__](x: __) -> __: ...
def h[_T_](x: _T_) -> _T_: ...
def i[__T__](x: __T__) -> __T__: ...
```

---

_@AlexWaygood reviewed on 2025-02-04 15:29_

---

_@ntBre reviewed on 2025-02-04 15:37_

---

_Review comment by @ntBre on `crates/ruff/tests/snapshots/lint__pep695_generic_rename.snap`:1 on 2025-02-04 15:37_

Good call, I copied my (poor) example from the test above.

---

_@ntBre reviewed on 2025-02-04 16:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/renamer.rs`:416 on 2025-02-04 16:24_

oh good idea! I also moved `try_shadowed_kind` to an associated function `ShadowedKind::new`. Definitely open to a better name there or reverting to a standalone function!

---

_@ntBre reviewed on 2025-02-04 16:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:59 on 2025-02-04 16:36_

Oh good to know. The S606 example linked above does not have the quotes and renders correctly on the website, so I'll remove these to be safe.

---

_@ntBre reviewed on 2025-02-04 16:38_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:79 on 2025-02-04 16:38_

Do you ever make the `Violation` type an enum itself? I just tried that locally and it seems to be working:

```rust
#[derive(ViolationMetadata, Debug, Copy, Clone, PartialEq, Eq)]
pub(crate) enum PrivateTypeParameter {
    Class,
    Function,
}
```

but I'm not sure if that will cause problems somewhere.

I'll just go with your suggestion here.

---

_@AlexWaygood reviewed on 2025-02-04 16:41_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:79 on 2025-02-04 16:41_

hmm, dunno! That could work! @MichaReiser redesigned our `ViolationMetadata` macro recently; it's possible that it wasn't possible to do that before, but it now is. Not sure!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/used_dummy_variable.rs`:193 on 2025-02-04 17:03_

I know it's not an issue that you're introducing here, but I'd expect a function with the name `get_possible_fix` to return a `Fix` rather than `Option<String>`. Maybe we could rename it to `get_possible_new_name` or similar?

---

_Marked ready for review by @ntBre on 2025-02-04 17:04_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:51 on 2025-02-04 17:05_

I would maybe move this up and make it a second paragraph in the "Why is this bad?" section, since it's also relevant for users who don't opt into autofixes

---

_@AlexWaygood approved on 2025-02-04 17:05_

Great work!!

---

_Comment by @ntBre on 2025-02-04 17:11_

Awesome, thanks for all of your help! Is UP051 an acceptable code to use? I just searched issues and PRs again, and the highest number I see now is UP048 (#15625). I think #15841 was previously UP050, which is probably where I got UP051. I'm happy to switch to UP049 to avoid leaving a gap, if you want.

---

_Comment by @AlexWaygood on 2025-02-04 17:15_

It's nice to avoid leaving a gap where possible with the rule codes, but I don't think it matters too much in cases like this where there's no corresponding "upstream" rule! It _is_ quite important to make sure we don't use the same code as a rule that we used to have but has since been deprecated or removed, however.

---

_@ntBre reviewed on 2025-02-04 17:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/used_dummy_variable.rs`:193 on 2025-02-04 17:29_

I came up with `get_candidate_name` and `try_get_name` too, but I like `get_possible_new_name`.

---

_@ntBre reviewed on 2025-02-04 17:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/private_type_parameter.rs`:51 on 2025-02-04 17:31_

Ah makes sense, I wasn't sure where to put it, but near the top does seem better.

---

_@AlexWaygood approved on 2025-02-04 18:16_

---

_Renamed from "[`pyupgrade`] Rename private type parameters in PEP 695 generics (`UP051`)" to "[`pyupgrade`] Rename private type parameters in PEP 695 generics (`UP049`)" by @ntBre on 2025-02-04 18:21_

---

_Merged by @ntBre on 2025-02-04 18:22_

---

_Closed by @ntBre on 2025-02-04 18:22_

---

_Branch deleted on 2025-02-04 18:22_

---

_Comment by @AlexWaygood on 2025-02-04 18:35_

Oh shoot, I forgot to say. Can we add a mention of this rule to the `## See also` sections in the docs for UP046, UP047 and UP040? The autofixes for those rules will work really well if this one is enabled as well

---

_Comment by @ntBre on 2025-02-04 18:37_

Ahhh, good catch. I meant to do that the whole time but forgot. I'll open a follow-up!

---
