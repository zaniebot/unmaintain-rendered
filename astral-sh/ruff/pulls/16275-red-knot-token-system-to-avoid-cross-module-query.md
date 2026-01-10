```yaml
number: 16275
title: "[red-knot] Token system to avoid cross-module query dependencies"
type: pull_request
state: closed
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
base: main
head: micha/prove-system-to-avoid-cross-module-query-dependencies
created_at: 2025-02-20T13:50:42Z
updated_at: 2025-02-28T09:21:10Z
url: https://github.com/astral-sh/ruff/pull/16275
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Token system to avoid cross-module query dependencies

---

_Pull request opened by @MichaReiser on 2025-02-20 13:50_

## Summary

This PR introduces a token based system to reduce accidental cross-module query dependencies. 

The basic idea is that we create a custom accessor for tracked struct fields that return an `AstNodeRef` and require an extra `query_file: File` argument. 
The `query_file` is the file of the enclosing query and we compare it against the tracked struct's scope to ensure it is the same. 

The basic idea here is: You should probably not access the node if don't know in which context your query runs or wrap your code in an extra query. 


Getting the file should generally be easy. E.g. `TypeInferenceBuilder` has a `file` method. 

This system doesn't prevent abuse. It's normally easy to get the correct file. E.g. you can call `definition.scope(db).file(db)` to get the file
but let's not do that ;). 

The main downside of this approach is that it's sort of annoying. But I take that for extra peace of mind.


This should mitigate/fix https://github.com/astral-sh/ty/issues/208


## Alternative approach

I considered changing `AstNodeRef::node` to take a `File` instead. But that quickly became annoying because we'd lose the `Deref` and `Debug` would also need a workaround. It also isn't `AstNodeRef::node` that's the problem, it's accessing the field on the tracked struct. That's why I opted for the custom-accessor on the tracked struct approach.

The ideal solution is to solve this problem by restructuring our modules and enforce it with visibility constraints but that's a bit more work.

---

_Label `internal` added by @MichaReiser on 2025-02-20 13:51_

---

_Label `red-knot` added by @MichaReiser on 2025-02-20 13:51_

---

_Marked ready for review by @MichaReiser on 2025-02-20 13:53_

---

_Review requested from @carljm by @MichaReiser on 2025-02-20 13:53_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-20 13:53_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-20 13:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:41 on 2025-02-20 14:04_

Should the WARNING message above this field now be changed to something like "It's almost always incorrect to access this field diretcly; instead, go via the public `kind()` accessor getter"?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:58 on 2025-02-20 14:05_

```suggestion
    /// It acts as a token of proof that we aren't accessing an AST node from a different file
    /// to the one from which the current enclosing Salsa query is being called.
    /// Doing so would lead to cross-file dependencies, hurting incremental computation.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:183 on 2025-02-20 14:10_

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:58 on 2025-02-20 14:11_

```suggestion
    /// It acts as a token of proof that we aren't accessing an AST node from a different file
    /// to the one from which the current enclosing Salsa query is being called.
    /// Doing so would lead to cross-file dependencies, hurting incremental computation.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:210 on 2025-02-20 14:12_

```suggestion
/// `query_file` is the file for which the current query performs type inference.
/// It acts as a token of proof that we aren't accessing an AST node from a different file
/// to the one from which the current enclosing Salsa query is being called.
/// Doing so would lead to cross-file dependencies, hurting incremental computation.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/unpack.rs`:69 on 2025-02-20 14:13_

```suggestion
    /// It acts as a token of proof that we aren't accessing an AST node from a different file
    /// to the one from which the current enclosing Salsa query is being called.
    /// Doing so would lead to cross-file dependencies, hurting incremental computation.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/unpack.rs`:58 on 2025-02-20 14:14_

```suggestion
    /// It acts as a token of proof that we aren't accessing an AST node from a different file
    /// to the one from which the current enclosing Salsa query is being called.
    /// Doing so would lead to cross-file dependencies, hurting incremental computation.
```

---

_@AlexWaygood approved on 2025-02-20 14:14_

---

_@MichaReiser reviewed on 2025-02-20 14:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:41 on 2025-02-20 14:24_

I think the fact that it's private should be good enough

---

_@AlexWaygood reviewed on 2025-02-20 14:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:41 on 2025-02-20 14:26_

oh good point, I missed that change

---

_Comment by @MichaReiser on 2025-02-20 17:12_

One caveat. This doesn't protect us fully from cross-module dependencies. For example, a function can still call `semantic_index` (or `ast_ids`) and introduce a cross-module dependency that way. But I think it helps a little?

---

_Renamed from "[red-knot] Token system to avoid cross-module query depndencies" to "[red-knot] Token system to avoid cross-module query dependencies" by @MichaReiser on 2025-02-20 17:48_

---

_Comment by @carljm on 2025-02-20 19:40_

I have mixed feelings about this. It feels like it's not protecting the boundary at the layer where we need to protect it in order to get the results we actually want (as mentioned above about `semantic_index` and `ast_ids`), and it introduces a fair amount of "noise" in order to provide protection that isn't quite at the right layer (which means I would still want to explore other solutions to astral-sh/ty#208).

Exactly where the "right" layer is is still an open question: it could be set at "anything below Type", which would be I think pretty conceptually clean but might result in too many queries needed? But it seems clear that it ought to be set "above" semantic index at the very least.

If you feel this alone would have avoided a significant number of the errors we've made in this area in the past, I'm not opposed to merging it for now and considering other approaches later.

---

_Comment by @MichaReiser on 2025-02-21 07:30_

> If you feel this alone would have avoided a significant number of the errors we've made in this area in the past, I'm not opposed to merging it for now and considering other approaches later.

I do think it would have caught the cross-module use in `Class::implicit_instance_attribute` and in visibility constraint's `evaluate`. 

But I agree that it's not as good as I first thought it would be. 

---

_Closed by @MichaReiser on 2025-02-28 09:21_

---
