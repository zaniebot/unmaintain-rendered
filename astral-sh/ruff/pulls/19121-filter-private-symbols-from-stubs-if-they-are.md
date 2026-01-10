```yaml
number: 19121
title: Filter private symbols from stubs if they are internal types
type: pull_request
state: merged
author: zanieb
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: zb/filter-completions
created_at: 2025-07-03T13:57:07Z
updated_at: 2025-07-11T14:42:38Z
url: https://github.com/astral-sh/ruff/pull/19121
synced_at: 2026-01-10T18:33:12Z
```

# Filter private symbols from stubs if they are internal types

---

_Pull request opened by @zanieb on 2025-07-03 13:57_

This implements filtering of private symbols from stub files based on type information as discussed in https://github.com/astral-sh/ruff/pull/19102. It extends the previous implementation to apply to all stub files, instead of just the `builtins` module, and uses type information to retain private names that are may be relevant at runtime.

---

_Label `server` added by @zanieb on 2025-07-03 13:57_

---

_Label `ty` added by @zanieb on 2025-07-03 13:57_

---

_Comment by @github-actions[bot] on 2025-07-03 14:00_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-     memo fields = ~45MB
+     memo fields = ~41MB

Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~54MB
+     memo fields = ~49MB

werkzeug (https://github.com/pallets/werkzeug)
-     memo fields = ~142MB
+     memo fields = ~129MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~156MB
+     memo fields = ~142MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
-     memo fields = ~171MB
+     memo fields = ~156MB

vision (https://github.com/pytorch/vision)
- TOTAL MEMORY USAGE: ~368MB
+ TOTAL MEMORY USAGE: ~334MB

mkdocs (https://github.com/mkdocs/mkdocs)
-     memo fields = ~106MB
+     memo fields = ~97MB

tornado (https://github.com/tornadoweb/tornado)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB

cwltool (https://github.com/common-workflow-language/cwltool)
-     memo fields = ~228MB
+     memo fields = ~207MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

```
</details>


---

_Comment by @zanieb on 2025-07-03 14:03_

I'd appreciate suggestions for more test cases, if people know of them.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:445 on 2025-07-03 14:22_

```suggestion
        // Internal sunder items should be filtered out
        test.assert_completions_do_not_include("_T");
```

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:1 on 2025-07-03 14:26_

I think it would be great to include a multifile test that doesn't depend on typeshed. You can see an example here of how to write a multifile completions test:

https://github.com/astral-sh/ruff/blob/4f36f0677f09b0ed1ba20f583b6f017c4f7d26c8/crates/ty_ide/src/completion.rs#L2180-L2195

Tests that depend on exactly what symbols typeshed defines in specific files are quite likely to either break unexpectedly or silently stop testing what they were meant to be testing.

It would be great to have that test check that none of the following are included in autocomplete suggestions:
- A private TypeVar definition
- A private TypeVarTuple definition
- A private ParamSpec definition
- A private protocol definition
- A private type alias that is explicitly annotated with `typing.TypeAlias` (like this one: <https://github.com/python/typeshed/blob/2408c028f4f677e0db4eabef0c70e9f6ab33e365/stdlib/builtins.pyi#L240>)

---

_@AlexWaygood reviewed on 2025-07-03 14:26_

---

_Review comment by @zanieb on `crates/ty_ide/src/completion.rs`:1 on 2025-07-03 14:50_

Done in https://github.com/astral-sh/ruff/pull/19121/commits/74af9ee50b3a4702e72f2319c4cf63352c2f4351

---

_@zanieb reviewed on 2025-07-03 14:50_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:523 on 2025-07-03 14:52_

```suggestion
__mangled_name = 1
```

---

_@zanieb reviewed on 2025-07-03 14:52_

---

_Review comment by @zanieb on `crates/ty_ide/src/completion.rs`:537 on 2025-07-03 14:52_

@AlexWaygood why "A private type alias that is explicitly annotated with typing.TypeAlias"? It looks like the behavior doesn't change either way.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:550 on 2025-07-03 14:52_

```suggestion
        test.assert_completions_include("__mangled_name");
```

---

_@AlexWaygood reviewed on 2025-07-03 14:52_

nice!

---

_@zanieb reviewed on 2025-07-03 14:53_

---

_Review comment by @zanieb on `crates/ty_ide/src/completion.rs`:568 on 2025-07-03 14:53_

It could be nice to avoid the repetition here, but I don't love optimizing tests for that early. Thoughts?

---

_@zanieb reviewed on 2025-07-03 14:53_

---

_Review comment by @zanieb on `crates/ty_ide/src/completion.rs`:568 on 2025-07-03 14:53_

We could also just test a few of the "do not include" cases here.

---

_@zanieb reviewed on 2025-07-03 14:54_

---

_Review comment by @zanieb on `crates/ty_ide/src/completion.rs`:537 on 2025-07-03 14:54_

i.e., the naive alternative passes

```diff
diff --git a/crates/ty_ide/src/completion.rs b/crates/ty_ide/src/completion.rs
index 9a6190cb5..0ee759051 100644
--- a/crates/ty_ide/src/completion.rs
+++ b/crates/ty_ide/src/completion.rs
@@ -536,6 +536,9 @@ _private_type_var_tuple = TypeVarTuple("_private_type_var_tuple")
 public_explicit_type_alias: TypeAlias = Literal[1]
 _private_explicit_type_alias: TypeAlias = Literal[1]
 
+public_implicit_type_alias = Literal[1]
+_private_implicit_type_alias = Literal[1]
+
 class PublicProtocol(Protocol):
     def method(self) -> None: ...
 
@@ -558,6 +561,8 @@ class _PrivateProtocol(Protocol):
         test.assert_completions_do_not_include("_private_type_var_tuple");
         test.assert_completions_include("public_explicit_type_alias");
         test.assert_completions_include("_private_explicit_type_alias");
+        test.assert_completions_include("public_implicit_type_alias");
+        test.assert_completions_include("_private_implicit_type_alias");
         test.assert_completions_include("PublicProtocol");
         test.assert_completions_do_not_include("_PrivateProtocol");
     }
```

---

_@BurntSushi reviewed on 2025-07-03 15:06_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:568 on 2025-07-03 15:06_

For just two tests, I think this is fine to be honest. (And it seems like two cases is what we want.)

---

_Marked ready for review by @zanieb on 2025-07-03 15:11_

---

_Review requested from @carljm by @zanieb on 2025-07-03 15:11_

---

_Review requested from @sharkdp by @zanieb on 2025-07-03 15:11_

---

_Review requested from @dcreager by @zanieb on 2025-07-03 15:11_

---

_Review requested from @MichaReiser by @zanieb on 2025-07-03 15:11_

---

_@BurntSushi approved on 2025-07-03 15:13_

LGTM

---

_@AlexWaygood reviewed on 2025-07-03 15:14_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:537 on 2025-07-03 15:14_

oh whoops -- for some reason I thought that my patch excluded explicitly declared type aliases with private names. But you're quite right -- it doesn't do that. We'd probably need to add a new variant to the `DynamicType` enum to distinguish type-alias `Todo` types from other `Todo` types in order to get that working. (Shouldn't be too hard to do as a followup, I can pick it up.)

---

_@zanieb reviewed on 2025-07-03 15:19_

---

_Review comment by @zanieb on `crates/ty_ide/src/completion.rs`:537 on 2025-07-03 15:19_

I'll leave it to you then :) 

---

_Merged by @zanieb on 2025-07-03 15:19_

---

_Closed by @zanieb on 2025-07-03 15:19_

---

_Branch deleted on 2025-07-03 15:19_

---

_@AlexWaygood reviewed on 2025-07-11 14:42_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/completion.rs`:537 on 2025-07-11 14:42_

(I picked this up in https://github.com/astral-sh/ruff/pull/19282)

---
