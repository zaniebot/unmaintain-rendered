```yaml
number: 15122
title: "`@no_type_check` support"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/no-type-check
created_at: 2024-12-23T10:51:15Z
updated_at: 2024-12-30T09:43:31Z
url: https://github.com/astral-sh/ruff/pull/15122
synced_at: 2026-01-10T20:42:27Z
```

# `@no_type_check` support

---

_Pull request opened by @MichaReiser on 2024-12-23 10:51_

## Summary

This PR adds support for `@no_type_check`. Specifically, functions annotated with `@no_type_check`. 

I think I solved it and the below no longer applies :)

The basics are working, but I'm running into cycles when the diagnostics are emitted from the annotated function's signature and I want to get a second opinion on what the best approach. 

The current implementation infers the `FunctionType` and then checks if any decorator in `FunctionLiteralType::decorators` is the known `NoTypeCheck` function. This works well, but cycles are more likely because `FunctionLiteralType` infers the type of the return type and decorators, and any of those could create a cycle. 

An alternative is to do something [closer to Pyright](https://github.com/microsoft/pyright/blob/17cee31c838961c88103950d7a2d95399a2b6237/packages/pyright-internal/src/analyzer/decorators.ts#L54-L124):
Instead of resolving the entire `FunctionLiteralType`, iterate over the function's decorators directly and infer each decorator expression. This reduces the possible cycles because it reduces the scope to the decorators only. 


Either approach requires a `recovery_fn` on the called query to return `Type::Unknown` for the decorator's expression. So I think this PR might
be blocked on fixpoint iteration.  

## Test Plan

Added tests, some of them panic :) 


---

_Review requested from @carljm by @MichaReiser on 2024-12-23 10:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-23 10:51_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-23 10:51_

---

_Label `red-knot` added by @MichaReiser on 2024-12-23 10:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:41 on 2024-12-24 13:49_

Mypy should be all lowercase unless it starts a sentence:

```suggestion
Both mypy and Pyright flag the `unknown_decorator` but we don't. 
```

Also, wouldn't it be more correct to flag `unknown_decorator` here? This snippet desugars to something like this at runtime:

```py
from typing import no_type_check

def test() -> int:
    return a + 5

test = unknown_decorator(no_type_check(test))
```

So I would expect only the class/function and decorators beneath the `no_type_check` decorator to be covered by the `no_type_check` "magic", not decorators above the `no_type_check` decorator.

If this is something we'd ideally change at some point, could we add a note to that effect here?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:11 on 2024-12-24 13:50_

```suggestion
use ruff_python_ast as ast;
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:74 on 2024-12-24 13:56_

Could you say more about why you think pyright's partial support of this feature is desirable here? (Or if you don't think that pyright's behaviour is desirable here, should this be a TODO?)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1 on 2024-12-24 13:57_

nit: none of the changes to this file seem to be substantive; it looks like it's just refactoring of imports. Do they need to be part of this PR?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:481 on 2024-12-24 14:02_

- Elsewhere in red-knot we use the `into_` prefix for functions like this -- should we do the same here? E.g. https://github.com/astral-sh/ruff/blob/5bc9d6d3aa694ab13f38dd5cf91b713fd3844380/crates/red_knot_python_semantic/src/types.rs#L566-L571
- You can simplify the `expect_function` method a few lines above now that you've added this method:

    ```diff
    diff --git a/crates/red_knot_python_semantic/src/semantic_index/symbol.rs b/crates/red_knot_python_semantic/src/semantic_index/symbol.rs
	index 3e1ec2806..5ed9746e0 100644
	--- a/crates/red_knot_python_semantic/src/semantic_index/symbol.rs
	+++ b/crates/red_knot_python_semantic/src/semantic_index/symbol.rs
	@@ -463,10 +463,7 @@ impl NodeWithScopeKind {
	     }
	 
	     pub fn expect_function(&self) -> &ast::StmtFunctionDef {
	-        match self {
	-            Self::Function(function) => function.node(),
	-            _ => panic!("expected function"),
	-        }
	+        self.as_function().expect("Expected a function")
	     }
    ```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/context.rs`:151 on 2024-12-24 14:05_

```suggestion
                scope
                    .node()
                    .as_function()?
                    .range()
                    .contains_range(range)
                    .then_some(child_scope_id)
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:92 on 2024-12-24 14:15_

```suggestion
    # error: [unused-ignore-comment]
```

Also, I wonder if it would be nice to have an additional explanation included in the error message here, such as:

```suggestion
    # error: [unused-ignore-comment] "Unused ignore comment (all errors in `test` are already ignored due because it is decorated with `@no_type_check`)"
```

---

_@AlexWaygood reviewed on 2024-12-24 14:36_

Thanks! This overall seems like the way I would have gone about implementing this.

I _think_ I understand the issue you point out with regard to cycles -- correct me if I'm wrong:
1. This PR means that we need to evaluate the types for certain functions eagerly, in order to determine if they're decorated with `@no_type_check`, because we need to know if a function is decorated with `@no_type_check` in order to determine whether we should be emitting diagnostics regarding that function
2. But we emit diagnostics during the process of type inference, so some types aren't actually ready yet at the point when we emit diagnostics

Examining the types of the decorators directly (without inferring the type of the function that's being decorated) makes sense to me... or potentially we could filter out the `@no_type_check`-ignored diagnostics after the whole file has been inferred (rather than as part of `context.report_lint()`), because at that point we know that all types are "ready"?

---

_@MichaReiser reviewed on 2024-12-24 15:30_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:41 on 2024-12-24 15:30_

Thanks for the extra input. I intentionally left this open for now because the approach on how to implement this probably depends on how we handle those diagnostics :) I somewhat agree but then again, `no_type_check` disables type checking for the entire function... which I would expect includes all decorators.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:74 on 2024-12-24 15:32_

sure: It's mainly that the semantics are unclear according to [this discussion](https://discuss.python.org/t/no-type-check-decorator/43119) and the typing spec explicitly leaves it open for type checkers not to support classes.

---

_@MichaReiser reviewed on 2024-12-24 15:32_

---

_@MichaReiser reviewed on 2024-12-24 15:33_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1 on 2024-12-24 15:33_

IMO, splitting them out feels like too much work. I don't mind reverting 

---

_@MichaReiser reviewed on 2024-12-24 15:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:481 on 2024-12-24 15:34_

It's called `as` because it doesn't consume `self`, consistent with the terminology we use elsewhere. 



---

_@AlexWaygood reviewed on 2024-12-24 15:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:41 on 2024-12-24 15:35_

> but then again, `no_type_check` disables type checking for the entire function... which I would expect includes all decorators.

I would _not_ expect that, because the decorator is applied to the function (and in fact redefines the function!) after the function has actually been defined -- that's just how Python's semantics work. But maybe I'm unusual in "seeing through the sugar" when I read decorators in Python source code :-)

If this is something we might want to come back to later, maybe we could reword this like this:

```suggestion
Both mypy and Pyright flag the `unknown_decorator`. We don't currently, but we might consider doing so in the future.
```

---

_Comment by @MichaReiser on 2024-12-24 15:42_

> This PR means that we need to evaluate the types for certain functions eagerly in order to determine if they're 

I wouldn't necessarily say we evaluate them eagerly. It's still lazy. It's just that we need to infer the enclosing function's type when we emit a diagnostic. Other than that, yes, that's exactly the reason why

Filtering the diagnostics out at later stages is a possibility but it complicates the tracking of which ignore codes are used, because we may have to mark them as "unused" when filtering out diagnostics. That's why I prefer keeping it in the *emit* path as it is in this PR. I also think that will get the required functionality for free once we have fixpoint working. The only question is if we're concerned about handling cycles on a function level and, because of that, prefer infering the decorators instead (to have it at the expression level). 

A third possibility is that we track more state in the `InferContext` and have a `in_no_type_check` field that is one of:

* Yes: Set when inferring the function definition and it sees a `no_type_check` decorator
* IfEnclosing: Only in a `no_type_check` block if the enclosing function is decorated


That might actually be a good solution and might remove the need for some of the "search the child scopes to find the right function" code as well. It doesn't remove the need for fixpoint entirely. We might still run into cycles but it should be very unlikely. 

---

_@AlexWaygood reviewed on 2024-12-24 15:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:74 on 2024-12-24 15:42_

Ah, I forgot that mypy also didn't support decorating classes with `@no_type_check` (and in fact, no other type checker does currently). Your wording here made me think that we were specifically copying pyright's implementation decisions here, but that's obviously not the case. Could you maybe reword this to something like this?

```suggestion
Red Knot does not support `no_type_check` annotations on classes currently. The behaviour of `no_type_check` when applied to classes is [not fully specified currently](https://typing.readthedocs.io/en/latest/spec/directives.html#no-type-check), and applying the decorator to classes is not supported by any other type checkers currently.
```

We could even consider emitting a diagnostic if a class is decorated with `@no_type_check`, since it clearly won't have the effect the user wants.

---

_@MichaReiser reviewed on 2024-12-24 15:43_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:41 on 2024-12-24 15:43_

WIth later: I mainly mean, once we figured out how to implement this properly 😆 


---

_@MichaReiser reviewed on 2024-12-24 15:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:74 on 2024-12-24 15:44_

> We could even consider emitting a diagnostic if a class is decorated with @no_type_check, since it clearly won't have the effect the user wants.

Possibly, but not as part of this PR ;)

---

_@AlexWaygood reviewed on 2024-12-24 15:45_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1 on 2024-12-24 15:45_

I guess I'm okay with the changes here staying in the PR. It just took me a little longer to review the PR because I had to figure out if they were actually substantive or not :-)

---

_@AlexWaygood reviewed on 2024-12-24 15:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:74 on 2024-12-24 15:47_

> Possibly, but not as part of this PR ;)

Is it worth adding a TODO comment somewhere as part of this PR? ;)

---

_@MichaReiser reviewed on 2024-12-24 16:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:92 on 2024-12-24 16:09_

> Also, I wonder if it would be nice to have an additional explanation included in the error message here, such as:

Possibly, but it requires some extra metadata tracking for suppressions or at least post-processing in the `unused-ignore-comment` rule. I don't think it's a priority right now (we already go beyond by flagging unused ignore comments

---

_Comment by @github-actions[bot] on 2024-12-24 16:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:49 on 2024-12-24 16:44_

This behavior doesn't match my reading of the wording in the spec. "...all type errors for the def statement..." seems like it would include all decorators, regardless of position. (Or possibly exclude all decorators, regardless of position, depending whether you consider decorators part of the `def` statement or not; it seems to me that they are part of it.)

Nonetheless, I think we should go ahead with this order-dependent behavior for now, because I think it does make the most intuitive sense. Decorators preceding `@no_type_check` are outside the "scope" of `@no_type_check`, decorators after it are inside its "scope".

(From my experiments, it seems that both pyright and mypy never suppress errors in decorators?)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:91 on 2024-12-24 16:50_

nit: it's not specified at all :)

```suggestion
[not specified currently](https://typing.readthedocs.io/en/latest/spec/directives.html#no-type-check),
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:92 on 2024-12-24 16:51_

nit: it's not supported in mypy either.

```suggestion
and applying the decorator to classes is not supported by Pyright or mypy.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:94 on 2024-12-24 16:57_

"annotation" is the wrong terminology here; only things like `x: int` constitute annotations in Python

```suggestion
A future improvement might be to emit a diagnostic if `no_type_check` is used to decorate a
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:113 on 2024-12-24 17:00_

could we assert the error message here, so that it's obvious in the future what's changing if we decide to improve the diagnostic in the future?

```suggestion
    # error: [unused-ignore-comment] "Unused `knot: ignore` directive: 'unresolved-reference'"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/context.rs`:189 on 2024-12-24 17:05_

The name of this variant is quite verbose but doesn't really add much clarity IMO (it wasn't at all obvious at the usage sites what this variant represented; I still had to jump to the enum definition and read the doc comment to understand what it meant). I'd probably just call it `Maybe` or `Possibly`. The doc comment is great!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1062 on 2024-12-24 17:06_

nit: there's only one usage of `decorator_tys` after this point, shadowing it seems unnecessary; why not just change `decorator_tys` to `decorator_tys.into_boxed_slice()` below?

---

_@carljm approved on 2024-12-24 17:09_

Looks great! Nice solution to the cycle issues.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:1 on 2024-12-24 17:09_

nit: we use snake_case rather than kebab-case for most of our other mdtest filenames (and the decorator is called `no_type_check` rather than `no-type-check`, so snake case would make more sense to me here even if that wasn't our convention)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/context.rs`:38 on 2024-12-24 17:11_

I would probably call this field

```suggestion
    no_type_check_status: InNoTypeCheck,
``` 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/context.rs`:131 on 2024-12-24 17:13_

It seems like we currently only ever set this field to the `Yes` variant, so I'd be tempted to do something like this, which feels more readable to me:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/context.rs b/crates/red_knot_python_semantic/src/types/context.rs
index 9543bde55..ace859425 100644
--- a/crates/red_knot_python_semantic/src/types/context.rs
+++ b/crates/red_knot_python_semantic/src/types/context.rs
@@ -126,8 +126,8 @@ impl<'db> InferContext<'db> {
         });
     }
 
-    pub(super) fn set_in_no_type_check(&mut self, no_type_check: InNoTypeCheck) {
-        self.no_type_check = no_type_check;
+    pub(super) fn record_no_type_check_block_entered(&mut self) {
+        self.no_type_check = InNoTypeCheck::Yes;
     }
 
     fn is_in_no_type_check(&self) -> bool {
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index fc4970462..27132d97c 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -72,7 +72,7 @@ use crate::unpack::Unpack;
 use crate::util::subscript::{PyIndex, PySlice};
 use crate::Db;
 
-use super::context::{InNoTypeCheck, InferContext, WithDiagnostics};
+use super::context::{InferContext, WithDiagnostics};
 use super::diagnostic::{
     report_index_out_of_bounds, report_invalid_exception_caught, report_invalid_exception_cause,
     report_invalid_exception_raised, report_non_subscriptable,
@@ -1033,7 +1033,7 @@ impl<'db> TypeInferenceBuilder<'db> {
             // and, if so, suppress any errors.
             if let Type::FunctionLiteral(function) = ty {
                 if function.is_known(self.db(), KnownFunction::NoTypeCheck) {
-                    self.context.set_in_no_type_check(InNoTypeCheck::Yes);
+                    self.context.record_no_type_check_block_entered();
                 }
             }
         }
```

But the advantage of your current version is that it's more extensible, so I don't have a strong opinion

---

_@AlexWaygood approved on 2024-12-24 17:14_

Nice!

---

_@MichaReiser reviewed on 2024-12-26 10:02_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:94 on 2024-12-26 10:02_

Eh, but your suggested wording (thanks!) on line 89 used annotations. That's why I used it here.

---

_@MichaReiser reviewed on 2024-12-26 10:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:49 on 2024-12-26 10:09_

Yes, it seems neither [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=05e1a1d26c29be3df8f98652e1aa3805) nor [Pyright](https://pyright-play.net/?strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMopgD68CApkQMYAWZlA1gFCMACArivYQO4pEAmdXAEMYuVoRKIKNOk0HBYZAM4wAFAEooAWgB8mFDABcjKGaggyMNiBRRhUANRQArKfPMJxUjNoNWHFxgvAJCIKLiCkqqmjr6qMbuZpbWtvZOrkA) suppress errors in decorators.

I think we should do the same, considering that this most closely matches your reading of the spec and is what existing type checkers do. Changing from what mypy and Pyright do to this order-dependent checking is also easier because it's relaxation. The other way would be a breaking change.

Whether it's most intuitive probably depends on your understanding of how decorators work. My intuition was that the decorator applies to the entire def statement, including all decorators, regardless of the ordering. But that's not what existing type checkers do. I'd then expect that decorators aren't part of the function definition.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:94 on 2024-12-26 10:11_

Oh, great point, the use of "annotation" on that line is also incorrect, and should be changed :-)

I didn't notice that you used the word "annotation" in that sentence when I made my suggestion in https://github.com/astral-sh/ruff/pull/15122#discussion_r1896836658; I guess I was concentrating on other things at the time. Sorry!

---

_@AlexWaygood reviewed on 2024-12-26 10:11_

---

_@AlexWaygood reviewed on 2024-12-26 10:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:49 on 2024-12-26 10:21_

Honestly, I would much rather we give this decorator the same semantics as any other decorator in the language has at runtime when it comes to the things the decorator applies to. We can have a debate over whether or not that's intuitive to people who aren't fully familiar with the semantics of decorators, but doing otherwise just feels really inconsistent to me. Decorators have very consistent semantics on this point in Python (the desugaring I gave in https://github.com/astral-sh/ruff/pull/15122#discussion_r1896765782), but we'd be saying we'd be treating this one decorator differently to any other decorator in the language (and, in fact, differently to how it works at runtime!). I'm honestly not sure how we'd justify the different semantics for this decorator in documentation or if we were responding to issue reports; it just doesn't make sense.

I think mypy and pyright got it wrong here, and we have a chance to do better.

---

_@MichaReiser reviewed on 2024-12-26 10:48_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:49 on 2024-12-26 10:48_

> I think mypy and pyright got it wrong here, and we have a chance to do better.

Maybe, but do we feel that strongly that it's worth introducing an incompatibility? What you propose is also not consistent with the specification (regardless of whatever reading you use). Overall I feel like consistency with other tools and the specification is more important than deviating because it could be an annoyance for users using multiple type checkers. It's also something that's very easy to change later (and probably so uncommon, that getting this perfect has very diminishing return)



---

_@carljm reviewed on 2024-12-26 19:11_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:49 on 2024-12-26 19:11_

I regret bringing this up 😆 

> considering that this most closely matches your reading of the spec

No, my reading of the spec wording is that it would suggest suppressing errors in all decorators (they are part of the def statement, because they modify it and they syntactically can't stand apart from it), which is the opposite of what mypy and pyright do.

I don't think there is a consensus here that we'd be deviating from, I just think it's an edge-case behavior that nobody ever thought about very carefully, either in the existing type checkers or in writing the spec.

I'm also not concerned about this being an adoption barrier or compatibility problem, since it would only cause us to suppress a few additional type errors in an unusual case; worst case someone might have to remove a `type: ignore`? I think there will be many more serious problems for anyone trying to check a codebase on multiple different type checkers; I don't think people do this, because it's too painful. Compatibility between type checkers in practice is all about libraries being able to specify public interfaces that can work for users of multiple type checkers; this issue doesn't affect that.

If this ever came up in discussion in the typing council, I would argue for specifying the "suppress errors in later decorators" behavior, and I suspect there would be support for that position, because it's the most sensible one given Python semantics.

---

_@AlexWaygood reviewed on 2024-12-26 19:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:49 on 2024-12-26 19:23_

> I think there will be many more serious problems for anyone trying to check a codebase on multiple different type checkers; I don't think people do this, because it's too painful.

It's pretty common for library authors to do this, because library authors often want users of their library to have a good experience whether their users use pyright or mypy. And it's also pretty common for people to use VSCode or pycharm as their IDE locally (which usually means using pyright or pycharm in their editor, even though there _are_ mypy plugins for both) but run mypy in CI. So I don't think it's true that "people don't do this".

But I agree that people find it painful. I also agree that this is a very minor incompatibility relative to the many other incompatibilities that already exist between mypy/pyright/pycharm, and that are likely to exist between mypy/red-knot, pyright/red-knot and pycharm/red-knot. And I agree that people are very unlikely to come across this incompatibility in practice, and that there are easy workarounds for people who do come across it.

---

_@MichaReiser reviewed on 2024-12-27 17:38_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:49 on 2024-12-27 17:38_

I agree that there are very few users that will be impacted by this change, and if so, it's most likely only that a violation now has been fixed (which might raise an `unused-type-ignore` warning). However, my concern is that every deviation from mypy and pyright increases the migration cost to Red Knot. Is it because they have to change their mental model or because there are more, fewer, or different violations. That's why I think we should pick our battles carefully because large code bases are likely to exercise many deviations, and every one of those will require manual changes when migrating to Red Knot (or using Red Knot side by side with another type checker). 

My other concern is simply that I think this approach is the least spec-compliant because you could read it as either including or excluding all decorators. But excluding only some seems to be the most far-fetched interpretation. 

That's why I still think leaving it just as is for now is the best and we can easily change it later (it only requires moving a single line of code). However, I also get the impression that you two feel somewhat strongly about this, and that's why I don't object to changing it (just tell me, no need for an explanation ;)). 


---

_@AlexWaygood reviewed on 2024-12-27 17:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no-type-check.md`:49 on 2024-12-27 17:59_

I honestly think it's a mistake to read the language of the spec too closely in situations like this. The aim is that the spec will eventually be precise in its language in all areas. But the spec is still very new, and still a work in progress in many places. I think there are many places in the spec where the language the spec uses could plausibly be understood to imply behaviours in certain edge cases which were never intended by authors of the spec.

I agree that compatibility with other type checkers is really important, so I really appreciate you arguing strongly for this! However, I still feel quite strongly that the order-dependent behaviour (where `@no_type_check` applies its magic to all decorators below `@no_type_check`, and the class, but no decorators above it) is the better one. If one thing is important for a Python type checker, it's for the tool to have a consistent and understandable model of Python's semantics with as few special cases as possible. So I _really_ dislike the idea of carving out a special case for this decorator so that it works differently to all other decorators in Python. In this case, I don't think that's outweighed by the compatibility costs, personally :-)

---

_Merged by @MichaReiser on 2024-12-30 09:42_

---

_Closed by @MichaReiser on 2024-12-30 09:42_

---

_Branch deleted on 2024-12-30 09:42_

---
