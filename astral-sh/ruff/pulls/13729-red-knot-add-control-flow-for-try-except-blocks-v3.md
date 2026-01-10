```yaml
number: 13729
title: "[red-knot] Add control flow for try/except blocks (v3)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: except-handler-3
created_at: 2024-10-13T15:52:36Z
updated_at: 2024-10-16T13:15:20Z
url: https://github.com/astral-sh/ruff/pull/13729
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Add control flow for try/except blocks (v3)

---

_Pull request opened by @AlexWaygood on 2024-10-13 15:52_

## Summary

This is a third attempt to add some understanding of exception-handler control flow to red-knot. It follows the previous attempts https://github.com/astral-sh/ruff/pull/13338 (which was very naive about `finally` blocks) and #13633 (which was overly _ambitious_ in how it attempted to handle `finally` blocks).

The discussion in #13633 concluded that we aren't yet ready to add an accurate understanding of `finally` suites to red-knot, as it will require double-visiting of `finally` suites in order to accurately model the semantics, and this violates some core assumptions made by the use-def map. As such, this PR rips out a lot of the infrastructure that #13633 added for specifically dealing with `finally` suites, and instead adds some copious TODO comments in `builder.rs` and the tests.

The rest of this summary section is copied from the summary section of #13633:

---

The semantics of `try`/`except` blocks are very complicated! I've written up a long document outlining all the various jumps control flow could take, which can be found [here](https://astral-sh.notion.site/Exception-handler-control-flow-11348797e1ca80bb8ce1e9aedbbe439d). I won't try to summarise that document in this PR description. But I will give a brief description of some of the ways I've attempted to model these semantics in this PR:

Abstractions for handling `try`/`except` blocks have been added to a new `builder` submodule, `builder/exception_handlers.rs`:
- `TryNodeContext` keeps track of state for a single `try`/`except`/`else`/`finally` block. Exactly what state we need to keep track of varies according to whether the node has a `finally` branch, and according to which branch of the `StmtTry` node we're currently visiting.
- `TryNodeContextStack` is a stack of `TryNodeContext` instances. For any given scope, `try` blocks can be arbitrarily nested; this means that we must keep a stack of `TryNodeContext`s for each scope we visit.
- `TryNodeContextStackManager` is a stack of `TryNodeContextStack`s. Whenever we enter a nested scope, a new `TryNodeContextStack` is initialised by the `TryNodeContextStackManager` and appended to the stack of stacks. Whenever we exit that scope, the `TryNodeContextStack` is popped off the stack of stacks.

The diff for this PR is quite large, but this is mostly tests. There aren't actually _that_ many tests, but they unfortunately need to be quite verbose. This is because we may add a more sophisticated understanding of exception handlers in the future (where we would understand that e.g. `x = 1` can never raise an exception), and I wanted the tests to be robust to this so that they wouldn't have to be rewritten when that happens. (This also helps readability of the tests, since we obviously know that `x = 1` can never raise exceptions.) To address this, I made sure to use assignments to function calls for testing places where a raised exception could cause a jump in control flow. This will be robust to future improvements, since it will always be the case that we will consider a function call capable of raising arbitrary exceptions.

## Test Plan

Tests have been added using `mdtest`, and can be found in `crates/red_knot_python_semantic/resources/mdtest/except_handler_control_flow.md`.


---

_Label `red-knot` added by @AlexWaygood on 2024-10-13 15:52_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-13 15:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-13 15:52_

---

_Comment by @AlexWaygood on 2024-10-13 15:56_

(https://github.com/astral-sh/ruff/commit/aacc4e7be912fefcb5f7d1d083dcf05db4ce4bb7 and onwards are the commits that differ from https://github.com/astral-sh/ruff/pull/13633; all commits prior to that are the same as in https://github.com/astral-sh/ruff/pull/13633.)

---

_Comment by @github-actions[bot] on 2024-10-13 16:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2024-10-14 16:23_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs`:41 on 2024-10-14 16:23_

Removing the `RefCell` and instead rely on compile time borrow checking doesn't seem to be difficult. I quickly tried it out locally and the only issue you run into is with `record_definition`. My recommendation there is to instead expose an `iter_mut` method on `TryNodeStack` and inline the logic into `builder.rs`. 

Removing the `RefCell` should also remove the need for `TryNodeContextStackRefMut` because you can just return a `&TryNodeContextStack` or `&mut TryNodeContextStack` based on the mutability that is needed.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs`:33 on 2024-10-14 16:24_

```suggestion
            "exit_scope() should never be called on an empty stack \
```

Nit: I think it would also be fine to make this a debug assertion.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs`:101 on 2024-10-14 16:27_

Is it important to know if it is `Some` or `None` because i dont' see that the code below makes use of the fact other than in an assertion. If not, consider simplifying this to a `Vec` and using `std::mem::take` to consume the value (or change `exit_try_block` to take `self`)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:806 on 2024-10-14 16:32_

```suggestion
                    self.flow_restore(pre_try_block_state);
```

---

_@MichaReiser reviewed on 2024-10-14 16:33_

Wow, the code makes this seem easy

This looks good but I would prefer if we avoid using `RefCell` in the context. I even think that regular borrowing will simplify the implementation by removing the need for the `MutRef` type.

---

_@AlexWaygood reviewed on 2024-10-15 19:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs`:101 on 2024-10-15 19:27_

Thanks, I think this was leftover complexity from the earlier version when I was trying (and failing) to handle `finally` blocks properly

---

_Comment by @AlexWaygood on 2024-10-15 20:26_

Thanks @MichaReiser! Managed to get rid of the RefCell 🎉

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-15 20:26_

---

_Comment by @codspeed-hq[bot] on 2024-10-15 20:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/except-handler-3)

### Merging #13729 will **not alter performance**

<sub>Comparing <code>except-handler-3</code> (4233ceb) with <code>main</code> (d25673f)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @AlexWaygood on 2024-10-15 20:37_

> Merging #13729 will **degrade performances by 22.53%**

I don't understand how the last commit (https://github.com/astral-sh/ruff/pull/13729/commits/21ab48139118dd94079dda3fe8aad495fa90f5dd) I pushed could have caused this. Nothing I did in that commit should have had any negative impact on performance, I don't think. This doesn't make any sense to me. (Edit: I tried squashing commits and force-pushing to trigger a rerun of the benchmarks; no difference ☹️)

---

_@AlexWaygood reviewed on 2024-10-15 22:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:242 on 2024-10-15 22:20_

Oh, pretty sure this is the reason for the perf regression. I'm now taking a snapshot for every definition, even if it doesn't occur inside an exception handler 🤦

Will fix tomorrow.

---

_@AlexWaygood reviewed on 2024-10-16 09:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:242 on 2024-10-16 09:05_

This diagnosis was correct. https://github.com/astral-sh/ruff/pull/13729/commits/24d139891efc6030ef0f3ec2eebd1999c2e6e831 fixed nearly all of the performance regression; it's down to 1-2% now.

---

_@AlexWaygood reviewed on 2024-10-16 09:06_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs`:41 on 2024-10-16 09:06_

I had to bring back the `RefCell` in https://github.com/astral-sh/ruff/pull/13729/commits/24d139891efc6030ef0f3ec2eebd1999c2e6e831. `record_definition()` is indeed the only place where normal borrowing rules are difficult to work with here, but I still don't see how to avoid using a `RefCell` internally :( I tried your suggestion of exposing an `iter_mut` method and inlining the logic into `builder.rs`, but I still couldn't make the borrow checker happy.

I have at least managed to remove `TryNodeContextStackRefMut`, however!

---

_Comment by @AlexWaygood on 2024-10-16 09:07_

Ready for re-review again!

---

_@MichaReiser reviewed on 2024-10-16 10:06_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs`:41 on 2024-10-16 10:06_

Yeah, the `self.flow_snapshot` makes this a bit awkward. There are two possibilities:

1. Pass the snapshot to `record_definition`

```patch
Subject: [PATCH] Update snapshot

The old snapshot was incorrect because the format range excluded the first (all empty) line. Now the first line gets formatted also.
---
Index: crates/red_knot_python_semantic/src/semantic_index/builder.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder.rs b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs	(revision 24d139891efc6030ef0f3ec2eebd1999c2e6e831)
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs	(date 1729073072841)
@@ -103,9 +103,9 @@
             .expect("Always to have a root scope")
     }
 
-    fn try_node_context_stack(&self) -> &TryNodeContextStack {
+    fn try_node_context_stack(&mut self) -> &mut TryNodeContextStack {
         self.try_node_context_stack_manager
-            .current_try_context_stack()
+            .current_try_context_stack_mut()
     }
 
     fn push_scope(&mut self, node: NodeWithScopeRef) {
@@ -239,7 +239,9 @@
             DefinitionCategory::Declaration => use_def.record_declaration(symbol, definition),
             DefinitionCategory::Binding => use_def.record_binding(symbol, definition),
         }
-        self.try_node_context_stack().record_definition(self);
+
+        let snapshot = self.flow_snapshot();
+        self.try_node_context_stack().record_definition(snapshot);
 
         definition
     }
Index: crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs b/crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs
--- a/crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs	(revision 24d139891efc6030ef0f3ec2eebd1999c2e6e831)
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs	(date 1729073033138)
@@ -23,6 +23,12 @@
             .expect("There should always be at least one `TryBlockContexts` on the stack")
     }
 
+    pub(super) fn current_try_context_stack_mut(&mut self) -> &mut TryNodeContextStack {
+        self.0
+            .last_mut()
+            .expect("There should always be at least one `TryBlockContexts` on the stack")
+    }
+
     /// Pop a new [`TryNodeContextStack`] off the stack of stacks.
     ///
     /// Each [`TryNodeContextStack`] is only valid for a single scope
@@ -38,33 +44,36 @@
 
 /// The contexts of nested `try`/`except` blocks for a single scope
 #[derive(Debug, Default)]
-pub(super) struct TryNodeContextStack(RefCell<Vec<TryNodeContext>>);
+pub(super) struct TryNodeContextStack(Vec<TryNodeContext>);
 
 impl TryNodeContextStack {
     /// Push a new [`TryNodeContext`] for recording intermediate states
     /// while visiting a [`ruff_python_ast::StmtTry`] node that has a `finally` branch.
-    pub(super) fn push_context(&self) {
-        self.0.borrow_mut().push(TryNodeContext::default());
+    pub(super) fn push_context(&mut self) {
+        self.0.push(TryNodeContext::default());
     }
 
     /// Pop a [`TryNodeContext`] off the stack.
-    pub(super) fn pop_context(&self) -> Vec<FlowSnapshot> {
+    pub(super) fn pop_context(&mut self) -> Vec<FlowSnapshot> {
         let TryNodeContext {
             try_suite_snapshots,
         } = self
             .0
-            .borrow_mut()
             .pop()
             .expect("Cannot pop a `try` block off an empty `TryBlockContexts` stack");
         try_suite_snapshots
     }
 
     /// For each `try` block on the stack, push the snapshot onto the `try` block
-    pub(super) fn record_definition(&self, builder: &SemanticIndexBuilder) {
-        for context in self.0.borrow_mut().iter_mut() {
-            context.record_definition(builder.flow_snapshot());
+    pub(super) fn record_definition(&mut self, snapshot: FlowSnapshot) {
+        for context in self.0.iter_mut() {
+            context.record_definition(snapshot.clone());
         }
     }
+
+    pub(super) fn iter_mut(&mut self) -> impl Iterator<Item = &mut TryNodeContext> {
+        self.0.iter_mut()
+    }
 }
 
 /// Context for tracking definitions over the course of a single
```

2. do the "move" trick

```rust
let use_def = self.current_use_def_map_mut();
        match category {
            DefinitionCategory::DeclarationAndBinding => {
                use_def.record_declaration_and_binding(symbol, definition);
            }
            DefinitionCategory::Declaration => use_def.record_declaration(symbol, definition),
            DefinitionCategory::Binding => use_def.record_binding(symbol, definition),
        }

        let mut try_stack_manager = std::mem::take(&mut self.try_node_context_stack_manager);
        for context in try_stack_manager.current_try_context_stack_mut().iter_mut() {
            context.record_definition(self.flow_snapshot());
        }
        self.try_node_context_stack_manager = try_stack_manager;

        definition
```

---

_@AlexWaygood reviewed on 2024-10-16 10:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder/except_handlers.rs`:41 on 2024-10-16 10:10_

Idea (1) is what I did last night and it caused a massive perf regression, because it means you end up taking a snapshot after _every_ definition, even if you're not visiting a node inside an exception handler!

I'll try out idea (2) and see if it works 

---

_@MichaReiser approved on 2024-10-16 10:28_

---

_Comment by @AlexWaygood on 2024-10-16 10:51_

I'll wait before merging in case @carljm wants to take another look first.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md`:23 on 2024-10-16 11:45_

```suggestion
the `try`/`except` block is therefore the union of the type at the end of the `try`
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md`:65 on 2024-10-16 11:49_

It feels excessive to explicitly refer back to this document explicitly here? We already linked it once at the top. To be honest, I'm not even sure we need to link it at all, because this "literate test suite" covers the topic so clearly, with examples.
```suggestion
as `except:`. There might have been an unhandled exception, but
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/control_flow.md`:50 on 2024-10-16 11:54_

The current narrative text tends to always discuss the final type, but not intermediate types. I feel like the possibility that control flow enters `except` without having seen all of the `try` block (i.e. the fact that `Literal[1]` is part of the type here) is pretty relevant and worth explicitly calling out.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:873 on 2024-10-16 12:11_

Nit: I don't think this requires rethinking any assumptions of the use-def map. In fact I think the use-def map is the one piece that likely _won't_ have to change at all to accommodate this. Rather, it requires rethinking some assumptions of our general semantic-indexing and type-inference approach, which assume that we visit each node exactly once.
```suggestion
                // This requires rethinking some fundamental assumptions semantic indexing makes.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:875 on 2024-10-16 12:12_

Again here, I'm not convinced we add value by linking this doc, again. I think the narrative in the test suite already provides an excellent overview of the `finally` topic, with examples, and will be easier to maintain and keep correct over time than an external Notion document.

---

_@carljm approved on 2024-10-16 12:16_

Very happy with where this ended up, for now! Awesome work.

Just a few nits, all related to test/comment text, none of them blocking.

---

_@AlexWaygood reviewed on 2024-10-16 12:21_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:875 on 2024-10-16 12:21_

The Notion document is more expansive and includes some examples and discussion that were not included in the test suite, either for brevity or because it's silly to add tests for things where all the relevant assertions would be TODOs. I agree with your point that there are too many links to it, but this one I'm inclined to keep.

---

_Merged by @AlexWaygood on 2024-10-16 13:03_

---

_Closed by @AlexWaygood on 2024-10-16 13:04_

---

_Branch deleted on 2024-10-16 13:04_

---

_Comment by @carljm on 2024-10-16 13:15_

Woohoo, congrats on getting this landed!!

---
