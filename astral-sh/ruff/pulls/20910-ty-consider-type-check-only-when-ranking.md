```yaml
number: 20910
title: "[ty] Consider `type_check_only` when ranking completions"
type: pull_request
state: merged
author: decorator-factory
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: df-type-check-only
created_at: 2025-10-16T04:02:53Z
updated_at: 2025-10-28T13:51:16Z
url: https://github.com/astral-sh/ruff/pull/20910
synced_at: 2026-01-12T15:57:12Z
```

# [ty] Consider `type_check_only` when ranking completions

---

_@decorator-factory_

## Summary

This pull request enhances `Type`s with information on whether they are "for type checking only", and changes autocompletion ranking so that 

This is the first step in solving https://github.com/astral-sh/ty/issues/816

- [x] TODO: I only implemented `@type_check_only` for classes, right now I'm figuring out how to do it for functions. It's a bit puzzling why this decorator supports functions in the first place. The only place I found it used successfully is `scipy-stubs`, where "imaginary methods" are used to make certain classes match certain protocols.

## Test Plan

Added a `import_type_check_only_lowers_ranking` test case, which used to fail.


---

_Comment by @github-actions[bot] on 2025-10-16 07:04_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-16 07:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "[ty] Consider `type_check_only` when ranking completions" to "[ty] WIP: Consider `type_check_only` when ranking completions" by @decorator-factory on 2025-10-16 07:26_

---

_Label `ty` added by @AlexWaygood on 2025-10-16 10:56_

---

_Comment by @AlexWaygood on 2025-10-16 10:57_

> TODO: I only implemented `@type_check_only` for classes, right now I'm figuring out how to do it for functions. It's a bit puzzling why this decorator supports functions in the first place. The only place I found it used successfully is `scipy-stubs`, where "imaginary methods" are used to make certain classes match certain protocols.

Fine to defer functions for now, IMO! I think the vast majority of uses are with classes. Let's get an MVP in first that we can iterate on in followups

---

_@decorator-factory reviewed on 2025-10-17 00:34_

---

_Review comment by @decorator-factory on `crates/ty_ide/src/completion.rs`:851 on 2025-10-17 00:34_

Maybe it's out of scope for this PR, but perhaps deprecated items should also ranked  last? (and crossed out in the autocompletion list)

<img width="544" height="93" alt="Image" src="https://github.com/user-attachments/assets/44234814-e418-40bd-b2de-1ce8b7efde14" />


---

_Marked ready for review by @decorator-factory on 2025-10-17 00:38_

---

_Review requested from @carljm by @decorator-factory on 2025-10-17 00:38_

---

_Review requested from @AlexWaygood by @decorator-factory on 2025-10-17 00:38_

---

_Review requested from @sharkdp by @decorator-factory on 2025-10-17 00:38_

---

_Review requested from @dcreager by @decorator-factory on 2025-10-17 00:38_

---

_Review requested from @MichaReiser by @decorator-factory on 2025-10-17 00:38_

---

_Renamed from "[ty] WIP: Consider `type_check_only` when ranking completions" to "[ty] Consider `type_check_only` when ranking completions" by @decorator-factory on 2025-10-17 00:39_

---

_@decorator-factory reviewed on 2025-10-17 00:41_

---

_Review comment by @decorator-factory on `crates/ty_python_semantic/src/types/function.rs`:1370 on 2025-10-17 00:41_

Was it an important detail that it's really defined in the module at runtime? I am not sure, but `type_check_only` should be the only exception, probably

---

_@decorator-factory reviewed on 2025-10-17 00:42_

---

_Review comment by @decorator-factory on `crates/ty_python_semantic/src/types.rs`:907 on 2025-10-17 00:42_

If there a way to avoid this somehow? Maybe `typing.type_check_only` could get the `TYPE_CHECK_ONLY` decorator flag when it's "inserted" into the typing module? I wasn't able to find where that happens

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:2627 on 2025-10-17 07:17_

Given that this is only used in the LSP context. Could we infer this lazilzy (we can cache it with a salsa query) instead of storing it on every class literal?

So the way this would work is that you make it a method on `ClassLiteral` that then iterates over all decorators too see if any of them are `type_check_only`.

---

_@MichaReiser reviewed on 2025-10-17 07:20_

Nice! This looks great to me. I've one question about whether we can compute the `is_type_check_only` flag lazily rather than storing it on every `ClassLiteral`. 

It would also be great if we could add an integration test for this. See https://github.com/astral-sh/ruff/blob/651f7963a7b5e0ce63e1edb2ea9c3a84e30f0034/crates/ty_completion_eval/README.md#L11 for how to do that.

---

_@MichaReiser reviewed on 2025-10-17 07:21_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:851 on 2025-10-17 07:21_

Yeah, agree. I think that sounds like a great change for another PR

---

_Label `server` added by @MichaReiser on 2025-10-17 07:21_

---

_@AlexWaygood reviewed on 2025-10-17 07:25_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:2627 on 2025-10-17 07:25_

> Given that this is only used in the LSP context.

We'll want to use it in other contexts in the future, though, e.g. we'll want to emit a diagnostic if a `type_check_only` symbol is imported in a `.py` file outside of an `if TYPE_CHECKING` block

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:349 on 2025-10-17 11:55_

This seems to mean that completions that would require an auto-import are always considered "not type-check-only". Here's a screenshot from a local playground build using your branch (to build the playground locally, use `cd playground && npm start --workspace ty-playground` from the Ruff repo root):

<img width="1510" height="344" alt="image" src="https://github.com/user-attachments/assets/5884ae56-4dc2-4a67-93a8-13abad7e13c3" />

`module.py` has these contents, so I would expect `Foo` to be ranked below `Foooo`:

```py
from typing import type_check_only

@type_check_only
class Foo: ...

class Foooo: ...
```

since that's what I get in other situations on your branch (which is great!):

<img width="1416" height="466" alt="image" src="https://github.com/user-attachments/assets/09e3d31c-74a3-49a4-85f7-394e1eaba3ae" />


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:907 on 2025-10-17 12:02_

This should do it:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 820da99a89..19d78133b3 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -2205,6 +2205,11 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
         let known_function =
             KnownFunction::try_from_definition_and_name(self.db(), definition, name);
 
+        // `type_check_only` is itself not available at runtime
+        if known_function == Some(KnownFunction::TypeCheckOnly) {
+            function_decorators |= FunctionDecorators::TYPE_CHECK_ONLY;
+        }
+
         let body_scope = self
             .index
             .node_scope(NodeWithScopeRef::Function(function))
```

---

_@AlexWaygood reviewed on 2025-10-17 12:03_

Nice, thank you!

---

_@AlexWaygood reviewed on 2025-10-17 12:04_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:349 on 2025-10-17 12:04_

if getting this right is difficult for auto-import completions, it's fine to defer it for now, but we should add a TODO comment here if so

---

_@decorator-factory reviewed on 2025-10-17 12:56_

---

_Review comment by @decorator-factory on `crates/ty_ide/src/completion.rs`:349 on 2025-10-17 12:56_

I think let's defer it for now :+1:
I added some TODOs in the integration test.

Right now deprecation and type-check-only-ness are stored on types, not symbols, which makes this difficult. (we'd have to eagerly infer types to produce autoimport suggestions if I understand it correctly)

---

_Comment by @decorator-factory on 2025-10-17 13:01_

> I've one question about whether we can compute the is_type_check_only flag lazily rather than storing it on every ClassLiteral

I think it is nicely symmetrical with storing `FunctionDecorators` on function types :smiley_cat: . It also allows pre-populating this flag in things we know to be type-check only like `@typing.type_check_only` itself. (the diff Alex suggested above)

This is something `ty` would have to check pretty much every time you use a class in a runtime context (like if you do `list(something)`), so I don't know which way is better for performance there

---

_@AlexWaygood reviewed on 2025-10-17 13:09_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:349 on 2025-10-17 13:09_

Hmm, so we'd have to infer the types of every symbol that could possibly be imported in order to provide autocomplete suggestions for a single `x = Fo<CURSOR>` situation... that would indeed undermine the "only lazily infer exactly what we need" architecture of ty 😆

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:349 on 2025-10-17 13:44_

> situation... that would indeed undermine the "only lazily infer exactly what we need" architecture of ty 😆

Yeah, let's wait for @BurntSushi on this, but we can definitely infer the type after we've done some filtering

---

_@MichaReiser reviewed on 2025-10-17 13:44_

---

_Comment by @MichaReiser on 2025-10-22 15:21_

@decorator-factory I'm not sure what the state of this PR is. Would you mind requesting re-review when it's ready for another round of feedback or you can put your PR in draft mode and change it to "ready for review" once it's ready for another round. 

Thank you

---

_Review requested from @AlexWaygood by @decorator-factory on 2025-10-22 16:46_

---

_Review requested from @MichaReiser by @decorator-factory on 2025-10-22 16:46_

---

_Comment by @decorator-factory on 2025-10-22 16:47_

I think it's ready to merge in the current state. As Alex said, the main use case for `@type_check_only` is classes/protocols that only exist at runtime, so we can fix the `TODO` tests for methods in a later PR

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/truth/import-deprioritizes-type_check_only/main.py`:12 on 2025-10-23 13:03_

I _believe_ the idea with the tests in this crate is that for autocomplete suggestions that don't achieve a great result currently, that's simply reflected in a lower score in `crates/ty_completion_eval/completion-evaluation-tasks.csv`. So that implies that we should uncomment these lines, and then when we improve our ranking in the future that will simply result in us having to update the score in that CSV file to reflect the fact that the ranking has improved.

---

_@AlexWaygood approved on 2025-10-23 13:04_

Thanks, and sorry for the delay in getting back to this! Looks good now, just one comment about the tests

---

_@decorator-factory reviewed on 2025-10-23 13:58_

---

_Review comment by @decorator-factory on `crates/ty_completion_eval/truth/import-deprioritizes-type_check_only/main.py`:12 on 2025-10-23 13:58_

I see, that's how it seems to work in the `numpy-array` and `scope-existing-over-new-import` cases indeed. Fixed in the latest commit

---

_Merged by @AlexWaygood on 2025-10-23 14:09_

---

_Closed by @AlexWaygood on 2025-10-23 14:09_

---

_@BurntSushi reviewed on 2025-10-28 13:41_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/truth/import-deprioritizes-type_check_only/main.py`:12 on 2025-10-28 13:41_

@AlexWaygood Yup that's exactly right! More broadly, it may be the case that some rankings can't improve or can't improve without regressing others.

---

_@BurntSushi reviewed on 2025-10-28 13:51_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:349 on 2025-10-28 13:51_

My loose plan at this point was to basically never infer the type of unimported symbols, by virtue of there being so many. It's true that we could try doing that after we've done filtering, but I fear this could still have a negative overall effect where we end needing to type check a substantial portion of third party code just by virtue of using completions. And I of course worry about the latency impact this will have on auto-import completions.

This is why the way auto-import works currently is to basically bypass any type checking and just pick up symbols straight from the AST. The way I would try to tackle `type_check_only` support here is to see if we can add it to that AST extraction bit and _not_ try to bring in type checking machinery for it.

---
