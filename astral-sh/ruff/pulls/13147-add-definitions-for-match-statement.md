```yaml
number: 13147
title: Add definitions for match statement
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/match-definition
created_at: 2024-08-29T07:40:30Z
updated_at: 2024-09-03T18:10:26Z
url: https://github.com/astral-sh/ruff/pull/13147
synced_at: 2026-01-10T21:38:32Z
```

# Add definitions for match statement

---

_Pull request opened by @dhruvmanila on 2024-08-29 07:40_

## Summary

This PR adds definition for match patterns.

## Test Plan

Update the existing test case for match statement symbols to verify that the definitions are added as well.


---

_Label `red-knot` added by @dhruvmanila on 2024-08-29 07:40_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/node_key.rs`:15 on 2024-08-30 13:23_

I want to change the struct name because it doesn't correspond to only nodes now that an identifier can be represented as a key.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:864 on 2024-08-30 13:24_

This delegation is required so that the expressions are recorded in the `UseDefMap`.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:832 on 2024-08-30 13:25_

If I'm not mistaken, I think the structural matching will be dependent on the type of the subject expression.

---

_@dhruvmanila reviewed on 2024-08-30 13:25_

---

_Marked ready for review by @dhruvmanila on 2024-08-30 13:26_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-30 13:26_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-30 13:26_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-30 13:26_

---

_Comment by @github-actions[bot] on 2024-08-30 13:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/node_key.rs`:37 on 2024-08-30 13:51_

There's not much in it, but I'd be tempted to do something like this (diff is relative to your branch):

```diff
diff --git a/crates/red_knot_python_semantic/src/node_key.rs b/crates/red_knot_python_semantic/src/node_key.rs
index 9683b0e7f..6aead4ce6 100644
--- a/crates/red_knot_python_semantic/src/node_key.rs
+++ b/crates/red_knot_python_semantic/src/node_key.rs
@@ -21,14 +21,21 @@ impl NodeKey {
     where
         N: Into<AnyNodeRef<'a>>,
     {
-        let node = node.into();
+        Self::from(node.into())
+    }
+}
+
+impl From<AnyNodeRef<'_>> for NodeKey {
+    fn from(node: AnyNodeRef) -> Self {
         NodeKey {
             kind: Kind::Node(node.kind()),
             range: node.range(),
         }
     }
+}
 
-    pub(super) fn from_identifier(identifier: &Identifier) -> Self {
+impl From<&Identifier> for NodeKey {
+    fn from(identifier: &Identifier) -> Self {
         NodeKey {
             kind: Kind::Identifier,
             range: identifier.range(),
diff --git a/crates/red_knot_python_semantic/src/semantic_index/definition.rs b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
index 75c95a4bd..a54dd67fc 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/definition.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
@@ -462,6 +462,6 @@ impl From<&ast::ParameterWithDefault> for DefinitionNodeKey {
 
 impl From<&ast::Identifier> for DefinitionNodeKey {
     fn from(identifier: &ast::Identifier) -> Self {
-        Self(NodeKey::from_identifier(identifier))
+        Self(NodeKey::from(identifier))
     }
 }
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:166 on 2024-08-30 13:57_

Maybe `pattern_root` might be a better name for this field? (And same for the equivalent field on `MatchPatternDefinitionKind`?)

---

_@AlexWaygood reviewed on 2024-08-30 13:59_

This seems reasonable to me, but @carljm will obviously be the better reviewer here

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:1099 on 2024-08-30 15:30_

nit: if you don't need the index for any other reason, you can just use the `use_def_map` salsa query directly.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:832 on 2024-08-30 20:59_

I'm curious how you see the pros and cons of this separate pattern visitor, vs including this visit functionality directly in the builder, with some context state stored on the builder when we are visiting a pattern (similar to `current_assignment`?

The need to explicitly delegate `visit_expr` (and potentially other things in future?) seems like a downside of the separate-visitor approach. There are advantages to maintaining the invariant that everything will get visited by the same visitor.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:166 on 2024-08-30 21:02_

It is a pattern, so I think `root_pattern` would be better than `pattern_root` if we did change the name. But tbh I think `pattern` is also fine; typically we'd call everything else a "sub-pattern" -- the top-level pattern is "the pattern."

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:299 on 2024-08-30 21:05_

I think we will also need to store a reference to the subject here; see comments on type inference side.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:832 on 2024-08-30 21:07_

Yes, it will, which means we need a reference to the subject stored in the pattern Definition as well.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:809 on 2024-08-30 21:17_

I think you mean `self.types.expression_ty(subject)` here (`self.index` is the semantic index, which doesn't have an `expression_ty` method.)

It's a bit misleading to say "available in ...", because it won't be, if we are inferring a single match pattern definition region, unless we ensure it gets put there. And we should only infer the type of the subject once, even if we have many match-pattern definitions referring to it. This is why we add the subject as a standalone expression in the semantic index; so that we can query its type directly using a cached Salsa query. So the approach we will want here is that we will call the Salsa query `infer_expression_types(db, expression)`, where we get `expression` (which is an `Expression` ingredient) from `self.index.expression(the_subject_ast_expr)`, and extend our own inference with the result. Once that's done then we can look up its type in `self.types.expression_ty(subject.scoped_ast_id())`. (This is all similar to what's currently done in `infer_assignment_definition`).

It's fine for all of this to remain a TODO in this PR! I would just re-word the comment a bit:
```suggestion
        // against the subject expression type (which we can query via `infer_expression_types`)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:835 on 2024-08-30 21:24_

In this method, anywhere we would have created a `Definition` in semantic indexing, we should be calling `self.infer_definition(...)` to lookup the Definition by node (identifier in this case) and infer types in that definition's region. This is what ensures we never double infer types: a given expression or definition is only inferred in one region, and "outer" regions always get access to those types by going through the cached Salsa query.

At least that's the design so far. This case is a little odd, because a match pattern definition is a very limited region (just an identifier) that doesn't actually include any expressions. Currently there is also an invariant that we maintain that doing scope-level inference will infer a type for every Definition in that scope, and have those types in its `self.types.definitions` map. But I actually don't think we rely on that invariant anywhere, like we do on the all-expressions-have-types invariant.

I still think for now it's probably best if we go ahead and do inference on all the match-pattern Definitions when we encounter them here? 

Looking at this now, I also think when we do proper type inference for match statements, we will need to create a new ingredient and Salsa query for inferring types on a pattern and matching it against a subject, otherwise we will end up inferring types on expressions within the pattern multiple times (once per definition in that pattern), which is something we want to avoid. That new query doesn't need to happen in this PR, but it would be good to add a TODO here, something like `// TODO Salsa query for inferring pattern types and matching against subject`.

---

_@carljm approved on 2024-08-30 21:37_

This looks good! Match statements are really tricky, and push the edges of the current design -- thanks for carefully thinking through how to handle them.

The main substantive change I think we need is that I think each match-pattern Definition needs to keep a reference to the subject of the match statement. And I think that scope-level inference should call `infer_definition_types` for the match-pattern definitions when they are encountered, though this one may not be strictly necessary, and I'm open to discussion on it.

Still approving anyway, since any of these things could also be done in a follow-up PR, with no harm done.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/node_key.rs`:15 on 2024-09-02 07:47_

For all I'm concerned, `Identifier` is a node. It has a value and range. It only lacks a `NodeKind` and a `AnyNode` implementation, but we could easily add that.

---

_@MichaReiser reviewed on 2024-09-02 07:47_

---

_@dhruvmanila reviewed on 2024-09-02 08:12_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/node_key.rs`:37 on 2024-09-02 08:12_

Yeah, I like this. I'll take that up as a follow-up. Thanks for suggestion

---

_Review request for @MichaReiser removed by @MichaReiser on 2024-09-02 08:29_

---

_@dhruvmanila reviewed on 2024-09-02 08:30_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:832 on 2024-09-02 08:30_

The main reason for me to keep the visitor separate was because of the state variables which includes both the `index` and `pattern`. Here, the `pattern` is the outermost pattern being visited and is required to store because of the recursive nature of the visitor pattern. One advantage of using a separate visitor is that the state is reset as soon as the work is done. 

> The need to explicitly delegate `visit_expr` (and potentially other things in future?) seems like a downside of the separate-visitor approach. There are advantages to maintaining the invariant that everything will get visited by the same visitor.

I didn't realize that this delegation would be required until a bit later when I noticed some tests failing. That said, I think it makes sense to maintain this invariant to avoid any future failure like the one I mentioned. We could create a separate `State` struct which maintains the necessary variables.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:809 on 2024-09-02 08:34_

That makes sense, thanks for the explanation. I'll also add a reference to `self.infer_assignment_definition` in the comment.

---

_@dhruvmanila reviewed on 2024-09-02 08:34_

---

_@dhruvmanila reviewed on 2024-09-02 08:41_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/node_key.rs`:15 on 2024-09-02 08:41_

Sounds good, I can add that as a follow-up.

---

_@dhruvmanila reviewed on 2024-09-02 08:48_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:832 on 2024-09-02 08:48_

I'll make this change as a follow-up.

---

_@dhruvmanila reviewed on 2024-09-02 08:48_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:835 on 2024-09-02 08:48_

> I still think for now it's probably best if we go ahead and do inference on all the match-pattern Definitions when we encounter them here?

This requires creating the definition which requires the identifier index in the root pattern. This means we need to use the visitor pattern / save context on the type inference builder. I avoided this for now because I wasn't sure exactly how the type inference was going to work here which itself is a separate task. Still, if you want I can take that up as a follow-up work to infer the definition types by storing the context on the builder.

---

_Comment by @codspeed-hq[bot] on 2024-09-02 08:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/match-definition)

### Merging #13147 will **degrade performances by 10.11%**

<sub>Comparing <code>dhruv/match-definition</code> (097e81c) with <code>main</code> (227fa4e)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/match-definition)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/match-definition` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-with-preview-rules[numpy/globals.py]` | 827.9 µs | 921 µs | -10.11% |


---

_Merged by @dhruvmanila on 2024-09-02 09:10_

---

_Closed by @dhruvmanila on 2024-09-02 09:10_

---

_Branch deleted on 2024-09-02 09:10_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:832 on 2024-09-02 09:40_

https://github.com/astral-sh/ruff/pull/13209

---

_@dhruvmanila reviewed on 2024-09-02 09:40_

---

_@carljm reviewed on 2024-09-03 17:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:835 on 2024-09-03 17:00_

I'm not sure I understand "This requires creating the definition which requires the identifier index in the root pattern" -- I wasn't suggesting to fully do the inference here, just to actually make it go through the `infer_definition_types` query path (but then still end up with `Unknown` for now). But now that this is merged it's fine, we can follow up in a future PR on the inference part.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:835 on 2024-09-03 18:10_

I can look at it tomorrow morning. I might need to go through the discussion again to expand my comment ;)

---

_@dhruvmanila reviewed on 2024-09-03 18:10_

---
