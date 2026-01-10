```yaml
number: 19123
title: "[ty] Add initial implementation of goto definition for loads of local names"
type: pull_request
state: closed
author: Gankra
labels:
  - server
  - ty
assignees: []
draft: true
base: main
head: gankra/gotodef
created_at: 2025-07-03T15:15:09Z
updated_at: 2025-07-18T13:45:51Z
url: https://github.com/astral-sh/ruff/pull/19123
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Add initial implementation of goto definition for loads of local names

---

_Pull request opened by @Gankra on 2025-07-03 15:15_

_No description provided._

---

_Label `server` added by @Gankra on 2025-07-03 15:15_

---

_Label `ty` added by @Gankra on 2025-07-03 15:15_

---

_@Gankra reviewed on 2025-07-03 18:37_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_model.rs`:175 on 2025-07-03 18:37_

This is basically the happy path of `TypeInferenceBuilder::infer_place_load`. It's extremely limited because it can't walk into parent scopes, and because it never engages type inference, it can't reason about "oh this is `MyClass.myfield`".

I feel like the more Correct solution we want here is to augment `infer_place_load` to also yield Definition info, and then run the whole TypeInferenceBuilder machinery just like `inferred_type` does.

---

_@Gankra reviewed on 2025-07-03 18:37_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/call/bind.rs`:2670 on 2025-07-03 18:37_

Oops.

---

_@Gankra reviewed on 2025-07-03 18:38_

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/requests/goto_definition.rs`:21 on 2025-07-03 18:38_

Everything in this file is a copy of goto_type_definition but changed to goto_definition

---

_@Gankra reviewed on 2025-07-03 18:41_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:947 on 2025-07-03 18:41_

This is the first non-trivial property of the current implementation: the "definitions" we yield are the bindings that dominate the load. This is a *useful* answer but obviously not the only possible one.

---

_@Gankra reviewed on 2025-07-03 18:42_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:996 on 2025-07-03 18:42_

(in this case we point to both `ab = 1` (above) and `ab = 2` as both bindings can affect the load)

---

_@Gankra reviewed on 2025-07-03 18:43_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:1069 on 2025-07-03 18:43_

In this case there's no bindings for `ab` so we fail to yield anything (suboptimal).

---

_@Gankra reviewed on 2025-07-03 18:46_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:1192 on 2025-07-03 18:46_

Any kind of Store completely falls over right now -- it seems like most of the code gives up when it sees Store, even if it's also a Load, but maybe it's handled by different paths? (in the playground `ab += 2` does seem to support getting the type of `ab`)

---

_@Gankra reviewed on 2025-07-03 18:48_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:1221 on 2025-07-03 18:48_

Note that this is only working because the class is defined in the same scope. If `x = AB(5)` was in a function it would fail to resolve, because we don't look into parent scopes. yet.

---

_@Gankra reviewed on 2025-07-03 18:48_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:1257 on 2025-07-03 18:48_

This one is hopeless without type info.

---

_@Gankra reviewed on 2025-07-03 18:49_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:1301 on 2025-07-03 18:49_

Similarly hopeless without type info

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:1364 on 2025-07-03 18:50_

Globals of course fail here without processing of parent scopes.

---

_@Gankra reviewed on 2025-07-03 18:50_

---

_Comment by @github-actions[bot] on 2025-07-03 23:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/goto_definition.rs`:57 on 2025-07-08 11:33_

This should use the `linkSupport` value from the `DefinitionClientCapabilities` instead: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_definition

You'll need to add a new `definition_link_support` field in `ResolvedClientCapabilities` which takes it's value from `document.definition.link_support`. Refer to how `type_definition_link_support` is being computed.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/semantic_model.rs`:207 on 2025-07-08 11:38_

I think we can include `ExprContext::Del` here as the following should go to the definition of `x`:

```py
def _():
    x = 1
    del x<CURSOR>
```

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/semantic_model.rs`:206 on 2025-07-08 11:39_

This might a bit tricky because the definition that corresponds to this expression itself should also be included in the list of definitions. For example, here both definitions should be included:

```py
def _():
    x = 1
    x<CURSOR> = 2
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/goto.rs`:1192 on 2025-07-08 11:51_

Yeah, augmented assignments are both `LOAD` and `STORE`.

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/semantic_model.rs`:205 on 2025-07-08 11:52_

Oh hmm, I see what you mean based on what you mentioned on Discord. So, this implementation would be limited to only provide definitions if they're defined in the current scope, right? The following doesn't work:

```py
x = 1


def _():
    x<CURSOR>
```

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/goto.rs`:58 on 2025-07-08 11:55_

Why do we require to call `unique` here?

---

_@dhruvmanila reviewed on 2025-07-08 12:15_

Thanks for taking this on! This seems like a good start but I'm a bit hesitant on shipping this based on the limitation that this only works if both the definition and usage are in the same scope.

The reason this limitation exists for goto definition and not goto type definition is because the former is only looking at the scope of the expression where it's being used while the latter is performing the type inference and fetching the list of definitions from `Type` instead (`Type::definition`). I think you're most likely aware of this already.

The reason it's fine for goto type definition is because we actually need the type definition which would either be in the stub file (if present) or the runtime file. This is the behavior of type inference in ty so it falls out naturally in the LSP.

For goto definition, I think we'll need something else. One way to solve this would be the following which is a bit high level:
1. This is something that Eric mentioned -- create a source mapping between the definitions in runtime and stub file
2. Use the same path as goto type definition but use the above mapping to get the corresponding runtime definition instead of the type definition

But to decrease the scope, we could probably ship a limited version which performs the same steps as goto type definition but filters out all the targets that would lead to a stub file and only return the ones that are present in the runtime files. This does mean that goto definition on any builtin symbols or the ones originating in the standard library wouldn't work because they all go to the stub files present in typeshed.

---

_@dhruvmanila reviewed on 2025-07-08 12:16_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/goto.rs`:1069 on 2025-07-08 12:16_

This would be part of goto declarations instead :)

https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_declaration

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/goto.rs`:33 on 2025-07-08 12:18_

I think we should split this into two files: `goto_type_definition.rs` and `goto_definition.rs` to isolate the logic and the tests. I'm not exactly sure where should the shared logic reside in, maybe the `goto.rs` could be used for that but that sounds a bit confusing.

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/goto.rs`:1416 on 2025-07-08 12:20_

We might want to create a generic `GotoDiagnostic` which takes in an enum that specifies which kind it is: `GotoKind::Definition`, `GotoKind::TypeDefinition`, `GotoKind::Declaration`, etc.

---

_@dhruvmanila reviewed on 2025-07-08 12:20_

---

_@Gankra reviewed on 2025-07-08 13:43_

---

_Review comment by @Gankra on `crates/ty_ide/src/goto.rs`:58 on 2025-07-08 13:43_

For whatever reason it's the constructor that takes a list (the name was concerning, but, it works?)

---

_@Gankra reviewed on 2025-07-08 13:44_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_model.rs`:205 on 2025-07-08 13:44_

Yes correct.

---

_@Gankra reviewed on 2025-07-08 13:45_

---

_Review comment by @Gankra on `crates/ty_server/src/server/api/requests/goto_definition.rs`:57 on 2025-07-08 13:45_

Hmm what distinguishes this from goto_type_definition, which has the same impl? (or is this a "also fix goto_type_definition"?)

---

_@MichaReiser reviewed on 2025-07-08 14:15_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:58 on 2025-07-08 14:15_

The idea behind using unique here is that, at least for go to type definition, we want to avoid showing the same definition twice. E.g. if you have a `Class<str> | Class<int>` type. We only want to list one target because both those type point to the generic `Class<T>`

---

_@dhruvmanila reviewed on 2025-07-08 14:39_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/goto_definition.rs`:57 on 2025-07-08 14:39_

These are the client capabilities, so if a client has link support for goto type definition but not for goto definition (for whatever reason), the server needs to respect that. These capabilities are provided to the server during initialization where we make note of all the things that the client supports through which the server needs to make certain decisions. This is one of them.

---

_@dhruvmanila reviewed on 2025-07-08 14:42_

---

_Review comment by @dhruvmanila on `crates/ty_ide/src/goto.rs`:58 on 2025-07-08 14:42_

Thanks, I think this makes sense for goto definitions as well.

---

_Comment by @MichaReiser on 2025-07-18 13:45_

I'll close this, given that ty now supports basic go to definition (which I think was based on this PR)

---

_Closed by @MichaReiser on 2025-07-18 13:45_

---
