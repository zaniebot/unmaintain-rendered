```yaml
number: 13338
title: "[red-knot] Add control flow for `try`/`except` blocks"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/except-flow
created_at: 2024-09-12T09:38:15Z
updated_at: 2024-10-04T18:19:34Z
url: https://github.com/astral-sh/ruff/pull/13338
synced_at: 2026-01-12T15:55:44Z
```

# [red-knot] Add control flow for `try`/`except` blocks

---

_@AlexWaygood_

## Summary

This PR adds control flow for `try`/`except`/`else`/`finally` blocks to red-knot.

The semantics of `try`/`except` blocks are as follows:
1. If an `except` branch is taken, this indicates that an exception in the `try` block caused that block to terminate early. This means that potentially 0 of the definitions in the `try` block were executed; or potentially all of them were; or potentially some number in between.
2. All `except` and `else` branches are mutually exclusive: if a single `except` or `else` branch is taken, it means none of the others were taken.
3. If an `else` branch was taken, it means that the `try` block was fully executed without any exceptions having occurred; we can in this branch count on all definitions in the `try` block having been executed.
4. If there is a `finally` branch, this branch is _always_ executed last, before completion of the block, regardless of whether the block has any `except` or `else` branches and (if so) which one was taken.

In order to model (1), two new fields are added to the `SemanticIndexBuilder`:
- A new boolean `visiting_try_block` flag is added, to keep track of whether we are visiting the `try` branch of a `try`/`except`/`else`/`finally` block.
- A new `try_block_definition_states` field is added, which is a `Vec<FlowSnapshot>`. If the `visiting_try_block` flag is set to `true`, the `add_definition()` method pushes a snapshot to this vec after each new definition is added.

## Test Plan

`cargo test`.

I believe I still may be missing some test coverage for nested `try` blocks. An early version of this PR looked like it had some subtle bugs there (where the visitor would no longer remember it was in a `try` block if it had finished visiting a `try` block inside a `try` block). But I couldn't find a test that would fail as a result of the apparent bug (which I've now fixed in this PR branch, anyway).

---

_Label `red-knot` added by @AlexWaygood on 2024-09-12 09:38_

---

_Review requested from @carljm by @AlexWaygood on 2024-09-12 09:38_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-12 09:38_

---

_Comment by @github-actions[bot] on 2024-09-12 09:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (error)</summary>
<p>

```
Failed to clone langchain-ai/langchain: error: RPC failed; curl 56 GnuTLS recv error (-54): Error in the pull function.
error: 1179 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-09-12 09:52_

The `tomllib` benchmark failures indicate that this would lead to too many false positives unless we also understand all of these patterns:

```py
def foo():
    try:
        x = 42
    except TypeError:
        return
    # x is guaranteed to be defined here

def bar():
    try:
        x = 42
    except TypeError:
        raise RuntimeError
    # x is guaranteed to be defined here

def baz():
    for i in range(5):
        try:
            x = 42
        except TypeError:
            continue
        # x is guaranteed to be defined here

def eggs():
    for i in range(5):
        try:
            x = 42
        except TypeError:
            break
        # x is guaranteed to be defined here
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:4726 on 2024-09-12 18:04_

Should there be a colon after `NameError`? Is this a typo or expected? Same for other test cases.

---

_@dhruvmanila reviewed on 2024-09-12 18:04_

---

_@AlexWaygood reviewed on 2024-09-12 18:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4726 on 2024-09-12 18:10_

thanks!!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4754 on 2024-09-12 18:10_

```suggestion
            except NameError:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4778 on 2024-09-12 18:10_

```suggestion
            except NameError:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4808 on 2024-09-12 18:11_

```suggestion
            except NameError:
```

---

_@AlexWaygood reviewed on 2024-09-12 18:11_

---

_Comment by @AlexWaygood on 2024-09-12 18:13_

> The `tomllib` benchmark failures indicate that this would lead to too many false positives unless we also understand all of these patterns:

I discussed this in person with @carljm -- for now, we'll defer fixing this, since it's a general issue that also applies to other kinds of control flow such as `if`/`else`. It's important to fix and it comes up much more often with `try`/`except` than with `if`/`else`, but it's orthogonal to what this PR is trying to fix, so it's best kept out of this PR. I'll open a followup issue for this specifically.

---

_Comment by @AlexWaygood on 2024-09-12 18:16_

> I believe I still may be missing some test coverage for nested `try` blocks. An early version of this PR looked like it had some subtle bugs there (where the visitor would no longer remember it was in a `try` block if it had finished visiting a `try` block inside a `try` block). But I couldn't find a test that would fail as a result of the apparent bug (which I've now fixed in this PR branch, anyway).

To be more specific here: the buggy thing I was doing in an early version of the PR was this:

```diff
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -725,7 +725,6 @@ where
                 // states during the `try` block before visiting the `except` blocks.
                 let pre_try_block_state = self.flow_snapshot();
                 let saved_definition_states = std::mem::take(&mut self.try_block_definition_states);
-                let visiting_nested_try_block = self.visiting_try_block;
 
                 // Visit the `try` block!
                 //
@@ -733,7 +732,7 @@ where
                 // in `self.try_block_definition_states` after each new definition is recorded.
                 self.visiting_try_block = true;
                 self.visit_body(body);
-                self.visiting_try_block = visiting_nested_try_block;
+                self.visiting_try_block = false;
```

I'm pretty sure that this was incorrect (I don't think it handles nested `try` blocks properly at all?), but couldn't find a test that actually demonstrated the bugginess.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4491 on 2024-09-12 18:25_

In principle I think these tests would be better if they asserted the inferred type of the name inside the except block, rather than at end of scope.

But that's quite a major pain to do in our current test framework, so I would just leave the tests as-is for now and wait for the new test framework with `reveal_type` :)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:750 on 2024-09-12 18:28_

Do we need to do any merging of the `try_block_definition_states` if there are nested try statements? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:751 on 2024-09-12 18:30_

Can we remove the clone from here? It seems to be the only reference to `pre_try_block_state`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:792 on 2024-09-12 18:31_

Nit. We could avoid this call for the last except handler because we restore the `post_try_block_state` on line 797 but I'm not sure if it's worth the complexity.

---

_@MichaReiser approved on 2024-09-12 18:32_

Uff, excepts are hard haha. Great work!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4798 on 2024-09-12 18:34_

I think a test that is missing here is something verifying that _within_ the `else` block, names bound in the `try` block are definitely bound. (This is along the same lines as the above comment; we're generally weak on tests demonstrating inferred types inside a particular block, rather than at end of scope.)

But as mentioned above, this is difficult to do with current test infra. (You can assign the name to another name, but then that other name is still potentially unbound, so the test isn't super clear.) But it would be nice to have a TODO to add this test, once we have the new test framework.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:751 on 2024-09-12 18:36_

Great catch. I think in an early version of the PR I thought I'd be referencing this again later, but obviously I don't now!

---

_@AlexWaygood reviewed on 2024-09-12 18:36_

---

_@carljm approved on 2024-09-12 18:38_

This is fantastic!! Probably the most complex control flow we'll need to handle, due to the "control flow can depart anywhere in the middle of a block" wrinkle.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:762 on 2024-09-13 04:10_

I think the exception to this is if we can guarantee that the inner try block catches all exceptions. I think the only way we can be sure of this is if it has a bare except (or maybe an `except BaseException`?). In this case the outer try shouldn't see all the possible partial states from the inner try, it should only see the states after the except or else. 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:762 on 2024-09-13 04:12_

Oh, and if the inner try has a finally block, the outer one should not see any state that doesn't include the finally block having run. So that's another thing we aren't quite doing right here yet. 

Good grief this is complex!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:167 on 2024-09-13 04:14_

I think it would be slightly better if this were kept as temporary state in the scope stack in the index builder, rather than in the scope itself. I think it is only needed transiently in the builder, so we don't need to pay the memory cost of storing it permanently in the symbol table for every scope. 

---

_@carljm reviewed on 2024-09-13 04:15_

---

_@AlexWaygood reviewed on 2024-09-13 13:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:167 on 2024-09-13 13:59_

I suspected I might get this feedback 😄 I decided to do this anyway as it seems much cleaner than the alternative. But I guess perf wins out!

---

_@AlexWaygood reviewed on 2024-09-13 14:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/symbol.rs`:167 on 2024-09-13 14:35_

I pushed https://github.com/astral-sh/ruff/pull/13338/commits/c25592d095833eb1c9f1d901e08439ee9b5d6e2e

---

_@AlexWaygood reviewed on 2024-09-13 17:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:762 on 2024-09-13 17:38_

> or maybe an `except BaseException`?

oh no... I don't know how we do that without type inference capabilities in the `SemanticIndexBuilder`?

---

_@AlexWaygood reviewed on 2024-09-13 17:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:762 on 2024-09-13 17:42_

For now I'll just handle the bare `except:` case, I think :/

---

_@carljm reviewed on 2024-09-13 17:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:762 on 2024-09-13 17:45_

> I don't know how we do that without type inference capabilities in the `SemanticIndexBuilder`?

Let's not worry about this wrinkle for this PR. I think handling bare except correctly is good; `except BaseException` is less common.

Eventually we will need to find a solution for adding conditional control flow paths that can be eliminated from consideration in type inference. It may be possible to do this as an extension of "constraints" using the Never type. We'll need this also for context manager `__exit__` return type handling.

---

_Comment by @AlexWaygood on 2024-09-13 17:48_

Noting it here for myself because I'm context-switching to something else: even not considering nested `try` blocks, the logic in this PR is currently incorrect. The logic currently assumes that at least one of the `except` and/or `else` branches must have been executed prior to the `finally` block being executed. That's only actually true if one of the `except` branches is a catch-all branch (either a bare `except` or an `except BaseException` branch). Otherwise, we could skip straight from the `try` block to the `finally` block in something like this if e.g. `RuntimeError` is raised during the `try` block:

```py
try:
    x = 1
except TypeError:
    x = 2
else:
    x = 3
finally:
    pass
```

This means that the inferred type of `x` needs to be `Unbound | Literal[1, 2, 3]` after this `try`/`except` block, rather than `Literal[2, 3]`.

---

_Comment by @AlexWaygood on 2024-09-13 22:03_

Re-requesting review as I had to change the approach quite radically to fix issues with nested `try`/`except` blocks and `finally` clauses!

---

_Review requested from @carljm by @AlexWaygood on 2024-09-13 22:04_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-13 22:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:101 on 2024-09-13 22:27_

I don't think callers of `push_scope` and `pop_scope` should be required to keep track of try block contexts data returned from `push_scope` and pass it back into `pop_scope`. Instead we can keep a per-scope stack of TryBlockContexts on the builder itself; `push_scope` can push onto it and `pop_scope` can pop off of it, and it can all be handled transparently to the caller.

(I'm pretty sure looking at this now that `loop_break_states` also has bugs with loops containing nested scopes containing loops, and needs this same scope-stack treatment! I'll fix this in a separate PR.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:774 on 2024-09-13 22:34_

This comment looks a bit outdated relative to the implementation?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:873 on 2024-09-13 22:42_

It could be relevant to perf. Snapshotting and restoring flow states is sufficiently expensive (if there are lots of symbols) that it might be worth explicitly checking for empty else and finally, if that would allow us to skip any snapshots or restores/merges. And it looks like we could skip a fair amount of those, if there is no finally body and no parent try block?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:868 on 2024-09-13 22:44_

Do we also need to merge the pre-try-block state here? (We could jump straight from before completing any statement in the try block, to executing the finally block and/or into an outer try block.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:880 on 2024-09-13 22:45_

I find it slightly confusing that within the same try block visit, above we refer to an outer try block as `parent_try_block` and here we refer to the same thing as `current_try_block`. Let's be consistent about what is "current" and what is "parent"?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1027 on 2024-09-13 22:47_

I like this change, letting us keep scope pushes and pops paired; I'd keep this, even if we no longer _need_ it in order to handle the try block contexts.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:242 on 2024-09-13 22:51_

We could check `visiting_nested_try_stmt` inside `TryBlockContext::push_snapshot` instead, to keep things a bit more encapsulated.

This would mean passing in `self` instead of `self.flow_snapshot()`, to avoid taking snapshots we won't use, but that seems fine.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:782 on 2024-09-13 22:53_

This is a lot of boilerplate we repeat several times. I think maybe we can reduce it somewhat by not using a RefCell, but we can also have a `self.current_try_block_mut()` wrapper method to reduce the repetition.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1175 on 2024-09-13 22:55_

It's not clear to me why we need to introduce a `RefCell` here. Maybe it's related to having all the scope pushing and popping methods holding onto references unnecessarily? I think if we don't do that, the ownership here should be pretty straightforward (owned by the builder, mutable reference to current block occasionally borrowed for very short time) and there shouldn't be any need for `RefCell`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4707 on 2024-09-13 22:57_

There is no `except TypeError` branch; out of date comment? Or maybe the test should be changed back from `except:` to `except TypeError:`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4701 on 2024-09-13 22:58_

It seems like removing these two lines wouldn't change the semantics of the test at all, so why include them?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4824 on 2024-09-13 23:03_

It's possible, just a bit of a pain. But on second thought, it's less of a pain than I'd originally thought. If you have a separate variable (let's say `reveal_x`) just for the purpose of inspecting a value in a nested location, and initialize it up top to a value nothing else is initialized to (`reveal_x = 0`), and then at the nested location where you want to inspect `x` you do `reveal_x = x`), then if you inspect `reveal_x` at the end you'll get a union of `Literal[0]` and the value of `x` where you wanted to inspect it. It's not quite as clear as a `reveal_type` in the new testing framework will be, but it's not bad. Up to you if you find that nicer than a TODO comment, if you're editing this PR anyway.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4888 on 2024-09-13 23:08_

I think `x` can be `Unbound` here; it's never set in an exhaustive `except`, or in a `finally`.

I think this might be the issue I pointed out above, where we don't include the pre-try-block state as a pre-finally possibility.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4916 on 2024-09-13 23:14_

I don't see how `x` could actually be unbound in this scenario, but I'm also not sure if it's worth fixing. We assume that we could exit the outer try with an exception before executing the inner try at all, but that's not actually true in this example; nothing happens in the outer try before we enter the inner try, so in actual fact we are guaranteed to either complete the assignment `x = 1` in the inner try, or have an exception caught by the exhaustive inner handler, which also binds `x`.

I think we would handle this correctly if, instead of saving a pre-try state, and then pushing a state post each definition, we instead pushed a state pre each definition, and then saved a post-try state.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4969 on 2024-09-13 23:18_

"otherwise-exhausive except/else branches in the inner block" doesn't seem right -- those aren't exhaustive

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4970 on 2024-09-13 23:20_

How is this assertion different from if the outer `try/except` weren't there?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:5001 on 2024-09-13 23:22_

How is this assertion different from if the outer `try/except` weren't there?

---

_@carljm reviewed on 2024-09-13 23:23_

Awesome work tracking down all these different test cases and sorting out how to make them all pass!

---

_@carljm reviewed on 2024-09-13 23:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:101 on 2024-09-13 23:43_

I looked into `loop_break_states`, and I think it's actually fine; since every nested loop resets it, and (unlike try/except) there's no interaction with the parent loop state, there's no potential for scope-confusion bugs.

---

_@AlexWaygood reviewed on 2024-09-14 16:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1027 on 2024-09-14 16:52_

I think maybe I'll separate this out into a separate PR, as I agree that it's a desirable change anyway, and it'll help reduce the diff of this PR

---

_@AlexWaygood reviewed on 2024-09-14 16:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:782 on 2024-09-14 16:55_

I agree that this is somewhat boilerplate-y. The version without the `RefCell` was unfortunately even more boilerplate-y, as it's hard to call a method on `TryBlockContexts` that takes `&mut self` in the same scope as a `self.flow_snapshot()` call, or the borrow checker complains that you have a mutable borrow and an immutable borrow in the same scope which is unsafe. Your suggestion in https://github.com/astral-sh/ruff/pull/13338#discussion_r1759593449 might fix this, however; I'll take a look.

---

_@AlexWaygood reviewed on 2024-09-14 16:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:4707 on 2024-09-14 16:55_

out-of-date comment, thanks!

---

_@AlexWaygood reviewed on 2024-09-14 16:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:101 on 2024-09-14 16:58_

> I don't think callers of `push_scope` and `pop_scope` should be required to keep track of try block contexts data returned from `push_scope` and pass it back into `pop_scope`. Instead we can keep a per-scope stack of TryBlockContexts on the builder itself; `push_scope` can push onto it and `pop_scope` can pop off of it, and it can all be handled transparently to the caller.

I'm okay with doing that, but how would that be different (in terms of memory usage) to keeping the `TryBlockContexts` state as a field on the `Scope` instances? I switched to this approach because of the concerns you stated in https://github.com/astral-sh/ruff/pull/13338#discussion_r1758151643

---

_@carljm reviewed on 2024-09-14 17:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:101 on 2024-09-14 17:04_

It's different because Scope is permanent; it's stored in the SemanticIndex and used in type inference. We should only put data in it that is needed by type inference. 

Storing state on the SemanticIndexBuilder is ephemeral, only kept while we are building the semantic index for a given module, and then dropped when that's done.

Keeping the state on the builder as I've suggested here is in fact equivalent in memory usage to what we're doing right now in this PR; either way the same number of TryBlockContexts are alive at any given time. The only difference is whether their ownership is held on the stack (the current approach in this PR) or as part of the builder. 

---

_@AlexWaygood reviewed on 2024-09-14 17:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:101 on 2024-09-14 17:04_

Ahh, thanks, that makes a lot of sense!

---

_@AlexWaygood reviewed on 2024-09-14 17:22_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1027 on 2024-09-14 17:22_

https://github.com/astral-sh/ruff/pull/13353

---

_@AlexWaygood reviewed on 2024-09-14 18:05_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1175 on 2024-09-14 18:05_

So if I try applying this diff:

<details>

```diff
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -96,7 +96,7 @@ impl<'db> SemanticIndexBuilder<'db> {
             .expect("Always to have a root scope")
     }
 
-    fn try_block_contexts(&self) -> &TryBlockContexts {
+    fn try_block_contexts(&mut self) -> &mut TryBlockContexts {
         self.try_block_contexts_stack.current_try_block_context()
     }
 
@@ -232,8 +232,7 @@ impl<'db> SemanticIndexBuilder<'db> {
             DefinitionCategory::Binding => use_def.record_binding(symbol, definition),
         }
 
-        if let Some(current_try_block) = self.try_block_contexts().borrow_mut().current_try_block()
-        {
+        if let Some(current_try_block) = self.try_block_contexts().current_try_block() {
             current_try_block.record_definition(self);
         }
 
@@ -780,9 +779,7 @@ where
                     .try_block_contexts()
                     .pop_try_block()
                     .expect("A `try` block should have been pushed to the stack prior to visiting the `body` field");
-                if let Some(parent_try_block) =
-                    self.try_block_contexts().borrow_mut().current_try_block()
-                {
+                if let Some(parent_try_block) = self.try_block_contexts().current_try_block() {
                     parent_try_block.record_visiting_nested_try_stmt();
                 }
                 self.flow_restore(pre_try_block_state);
@@ -877,9 +874,7 @@ where
                 // Account for the fact that we could be visiting a nested `try` block,
                 // in which case the outer `try` block must be kept informed about all the possible
                 // between-definition states we could have encountered in the inner `try` block(!)
-                if let Some(current_try_block) =
-                    self.try_block_contexts().borrow_mut().current_try_block()
-                {
+                if let Some(current_try_block) = self.try_block_contexts().current_try_block() {
                     current_try_block.record_exiting_nested_try_stmt();
                     current_try_block.record_definition(self);
                 }
diff --git a/crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs b/crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs
index de1918f71..b89bd4c52 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs
@@ -1,5 +1,3 @@
-use std::cell::{RefCell, RefMut};
-
 use super::SemanticIndexBuilder;
 use super::use_def::FlowSnapshot;
 
@@ -11,9 +9,9 @@ impl TryBlockContextsStack {
         self.0.push(TryBlockContexts::default());
     }
 
-    pub(super) fn current_try_block_context(&self) -> &TryBlockContexts {
+    pub(super) fn current_try_block_context(&mut self) -> &mut TryBlockContexts {
         self.0
-            .last()
+            .last_mut()
             .expect("There should always be at least one `TryBlockContexts` on the stack")
     }
 
@@ -52,28 +50,19 @@ impl TryBlockContext {
     }
 }
 
-#[derive(Debug)]
-pub(super) struct TryBlockContextsRefMut<'a>(RefMut<'a, Vec<TryBlockContext>>);
-
-impl<'a> TryBlockContextsRefMut<'a> {
-    pub(super) fn current_try_block(&mut self) -> Option<&mut TryBlockContext> {
-        self.0.last_mut()
-    }
-}
-
 #[derive(Debug, Default)]
-pub(super) struct TryBlockContexts(RefCell<Vec<TryBlockContext>>);
+pub(super) struct TryBlockContexts(Vec<TryBlockContext>);
 
 impl TryBlockContexts {
-    pub(super) fn push_try_block(&self) {
-        self.0.borrow_mut().push(TryBlockContext::default());
+    pub(super) fn push_try_block(&mut self) {
+        self.0.push(TryBlockContext::default());
     }
 
-    pub(super) fn pop_try_block(&self) -> Option<TryBlockContext> {
-        self.0.borrow_mut().pop()
+    pub(super) fn pop_try_block(&mut self) -> Option<TryBlockContext> {
+        self.0.pop()
     }
 
-    pub(super) fn borrow_mut(&self) -> TryBlockContextsRefMut {
-        TryBlockContextsRefMut(self.0.borrow_mut())
+    pub(super) fn current_try_block(&mut self) -> Option<&mut TryBlockContext> {
+        self.0.last_mut()
     }
 }
```

</details>

Then the borrow check complains thusly:

```
error[E0502]: cannot borrow `*self` as immutable because it is also borrowed as mutable
   --> crates/red_knot_python_semantic/src/semantic_index/builder.rs:236:49
    |
235 |         if let Some(current_try_block) = self.try_block_contexts().current_try_block() {
    |                                          ---- mutable borrow occurs here
236 |             current_try_block.record_definition(self);
    |                               ----------------- ^^^^ immutable borrow occurs here
    |                               |
    |                               mutable borrow later used by call

error[E0502]: cannot borrow `*self` as immutable because it is also borrowed as mutable
   --> crates/red_knot_python_semantic/src/semantic_index/builder.rs:879:57
    |
877 |                 if let Some(current_try_block) = self.try_block_contexts().current_try_block() {
    |                                                  ---- mutable borrow occurs here
878 |                     current_try_block.record_exiting_nested_try_stmt();
879 |                     current_try_block.record_definition(self);
    |                                       ----------------- ^^^^ immutable borrow occurs here
    |                                       |
    |                                       mutable borrow later used by call
```

LMK if you can see a way of getting round this without the `RefCell`... I agree it's less than ideal, for sure!

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/red_knot.rs`:57 on 2024-09-16 13:08_

I recommend moving this comment next to the relevant errors. It increases the likelihood that the comment gets deleted when the errors are resolved (I would miss the comment up there)


```suggestion
static EXPECTED_DIAGNOSTICS: &[&str] = &[
    "/src/tomllib/_parser.py:5:24: Module '__future__' has no member 'annotations'",
		
		// The failed import from 'collections.abc' is due to lack of support for 'import *'.
    "/src/tomllib/_parser.py:7:29: Module 'collections.abc' has no member 'Iterable'",
    
    "Line 69 is too long (89 characters)",
    "Use double quotes for strings",
    "Use double quotes for strings",
    "Use double quotes for strings",
    "Use double quotes for strings",
    "Use double quotes for strings",
    "Use double quotes for strings",
    "Use double quotes for strings",

				// The "possibly not defined" errors in `tomllib/_parser.py` are because
		// we don't yet understand terminal statements (`return`/`raise`/etc.) in our control-flow analysis
    "/src/tomllib/_parser.py:66:18: Name 's' used when possibly not defined.",
    "/src/tomllib/_parser.py:98:12: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:101:12: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:104:14: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:104:14: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:115:14: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:115:14: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:126:12: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:348:20: Name 'nest' used when possibly not defined.",
    "/src/tomllib/_parser.py:353:5: Name 'nest' used when possibly not defined.",
    "/src/tomllib/_parser.py:453:24: Name 'nest' used when possibly not defined.",
    "/src/tomllib/_parser.py:455:9: Name 'nest' used when possibly not defined.",
    "/src/tomllib/_parser.py:482:16: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:566:12: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:573:12: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:579:12: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:580:63: Name 'char' used when possibly not defined.",
    "/src/tomllib/_parser.py:629:38: Name 'datetime_obj' used when possibly not defined.",
];
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs`:1 on 2024-09-16 13:10_

I would make this a sub module of `builder` if it is only used in `builder.rs`. It allows to keep the structs scoped to `builder` (and not the entire `semantic_index`). 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs`:22 on 2024-09-16 13:11_

Nit. We should assert that the popped value is never `None` because that would indicate an unbalanced `push`/`pop`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs`:45 on 2024-09-16 13:12_

Nit: These methods arent' realy recording anything. Rename to `enter_nested_try_block` and `exit_nested_try_block`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:769 on 2024-09-16 13:18_

What's the reason that we need to "emulate" a stack here instead of making use of the function's stack to allocate the try blocks?

I looked at `TryBlockContextsStack` and I didn't spot anything that obviously requires a `Vec` (e.g. because we would need to inspect the entire stack.)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:786 on 2024-09-16 13:22_

The method name here seems misleading to me because it is called **after** we visit the `try` body. It seems it's about visiting the except handlers, finaly , and or else blocks of a `try`. 

Related: What's the reason that we don't record definitions in a nested try? E.g. what if

```python
try:
    try:
        x =  10:
        maybe_panic()
    finally:
        y = panic()
finally:
    y
```

shouldn't the use of `y` be an error? 

but 

```python
try:
    try:
        x =  10:
        maybe_panic()
    finally:
        y = 10
finally:
    y
```

should be okay?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:881 on 2024-09-16 13:26_

Can't we call `self.try_block_contexts_mut()` here to avoid the `RefCell` in the contexts (and all the `borrow_mut` stuff)?

---

_@MichaReiser reviewed on 2024-09-16 13:27_

To me it's unclear why we need the extra except handler stack vs just using the function stack (all you need is the current context and the parent's). Keeping an extra `Vec` around isn't expensive but I think it's slightly more complicated than just making use of the stack.

---

_Review requested from @MichaReiser by @MichaReiser on 2024-09-16 13:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs`:33 on 2024-09-16 14:36_

If I remove this check in the current version of this PR, all tests pass. So either we don't need this, or we need more tests.

---

_@carljm reviewed on 2024-09-16 14:36_

---

_@carljm reviewed on 2024-09-16 14:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:786 on 2024-09-16 14:42_

I commented elsewhere that it seems we currently don't have any tests that fail if we stop paying attention to `visiting_nested_try_stmt` entirely.

And it seems to me that Micha is right here -- outside the inner `try` block body itself, it does seem like intermediate states in the except/else/finally _should_ all get registered with the outer try block, because we could have an exception happen in any of those places. (And such an exception would not cause us to still enter an inner `finally` block, which only applies to the inner `try` body; it would jump straight to being handled by the outer `try` block.) So it does seem to me like `visiting_nested_try_stmt` might be inherently semantically not right?

---

_@carljm reviewed on 2024-09-16 14:44_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:769 on 2024-09-16 14:44_

I didn't understand this suggestion (or the similar one in the top-level comment) so I checked in with Micha off-line; it sounds like this suggestion was based on a misunderstanding of what this PR does (and needs to do), in terms of tracking stack of try-blocks per scope (i.e. it currently uses a nested vec of vecs). This comment is effectively suggesting to go back to the previous `std::mem::replace` approach, which isn't sufficient because it doesn't clear the try blocks on scope-push and restore them on scope-pop.

---

_@carljm reviewed on 2024-09-16 14:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1175 on 2024-09-16 14:48_

I'm fairly confident that there is a way to avoid the need for RefCell, and I also am interested in considering simpler representations than the current vec-of-vecs approach (e.g. can we use just a flat stack of try blocks, but via an enum that also has a no-try-block variant, so when we push scope we just push a no-try-block entry on the flat stack, and when we pop scope we pop it off?).

But I think this PR is complex enough that we should first make sure we have all the semantics and tests right, and _then_ worry about fine-tuning implementation strategy. So I suggest ignoring all implementation comments for now and focusing only on semantic comments until we are confident in the semantics and tests.

(For example, the approach for removing the need for RefCell will look different depending on whether we do or don't need `visiting_nested_try_stmt`, which seems in question to me at the moment.)

---

_@AlexWaygood reviewed on 2024-09-16 14:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs`:33 on 2024-09-16 14:54_

A test fails if you make this change:

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder.rs b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
index c7e8a6428..74cbd6b2d 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -873,16 +873,6 @@ where
                 }
 
                 self.visit_body(finalbody);
-
-                // Account for the fact that we could be visiting a nested `try` block,
-                // in which case the outer `try` block must be kept informed about all the possible
-                // between-definition states we could have encountered in the inner `try` block(!)
-                if let Some(current_try_block) =
-                    self.try_block_contexts().borrow_mut().current_try_block()
-                {
-                    current_try_block.record_exiting_nested_try_stmt();
-                    current_try_block.record_definition(self);
-                }
             }
             _ => {
                 walk_stmt(self, stmt);
diff --git a/crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs b/crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs
index 2eded0089..ce4e76ca0 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/except_handlers.rs
@@ -30,9 +30,7 @@ pub(super) struct TryBlockContext {
 
 impl TryBlockContext {
     pub(super) fn record_definition(&mut self, builder: &SemanticIndexBuilder) {
-        if !self.visiting_nested_try_stmt {
-            self.snapshots.push(builder.flow_snapshot());
-        }
+        self.snapshots.push(builder.flow_snapshot());
     }
```

The `if !self.visiting_nested_try_stmt` check isn't necessary for correctness. But it's unnecessary to push all those snapshots to be recorded by the outer `TryBlockContexts`, since it's more efficient -- and more correct -- to simply record the state once at the completion of the inner `try/except/else/finally` block. I.e., the `if !self.visiting_nested_try_stmt` check is simply an optimisation to avoid unnecessary snapshotting.

I see that at the moment this is quite unclear; I'll add some more comments.

---

_@carljm reviewed on 2024-09-16 15:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:786 on 2024-09-16 15:15_

(Cases to consider for tests here are e.g. `x = 1; x = 2` inside an `except`, `else`, or `finally` block in a nested try statement -- the `x = 1` possibility should be visible to the outer try block.

---

_@AlexWaygood reviewed on 2024-09-16 15:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:786 on 2024-09-16 15:18_

> (And such an exception would not cause us to still enter an inner `finally` block, which only applies to the inner `try` body; it would jump straight to being handled by the outer `try` block.)

What do you mean? The inner `finally` block is indeed always executed, even if the exception is caught by the outer `try` block:

```pycon
>>> try:
...     try:
...         raise TypeError()
...     except NameError:
...         print(1)
...     finally:
...         print(2)
... except TypeError:
...     print(3)
... finally:
...     print(4)
... 
2
3
4
```

---

_@carljm reviewed on 2024-09-16 15:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:786 on 2024-09-16 15:22_

What I mean is that if an exception is raised inside an except/else/finally block in the nested try (not inside the try body), in _that_ case the inner finally is not executed, the exception unwinds directly to the outer try statement handling.

---

_@carljm reviewed on 2024-09-16 15:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:786 on 2024-09-16 15:24_

... except this is just wrong 😆 apparently `finally` does still apply, even if the except is raised in an except handler or else block! So never mind.

---

_@carljm reviewed on 2024-09-16 15:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:786 on 2024-09-16 15:25_

I guess the one thing to consider still is the case where an exception is raised within the `finally` block itself? I.e. is it important that we register those intermediate states within the nested `finally` block with the outer try statement?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:786 on 2024-09-16 15:25_

I still can't find any cases where the inner `finally` is not executed, though it is of course true that if an exception is raised in an `except` handler then we might switch to the inner `finally` block before the except handler has finished, and indeed if an exception is raised in the inner `finally` block then that `finally` block might not run to completion. I might not handle _that_ properly right now...

```pycon
>>> try:
...     try:
...         raise TypeError
...     except TypeError:
...         raise RuntimeError
...         print(1)
...     finally:
...         print(2)
... except RuntimeError:
...     print(3)
... finally:
...     print(4)
... 
2
3
4
```

---

_@AlexWaygood reviewed on 2024-09-16 15:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4824 on 2024-10-02 20:37_

We now have `reveal_type`, so this is more easily doable, even though we don't have the full new testing framework yet

---

_@carljm reviewed on 2024-10-02 20:37_

---

_@carljm reviewed on 2024-10-02 20:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4916 on 2024-10-02 20:38_

I think this comment is wrong; given that we don't currently try to distinguish non-raising statements, we have to assume that `x = 2` in the inner exhaustive `except` could also raise, so it is correct for `x` to be possibly unbound.

---

_Comment by @AlexWaygood on 2024-10-04 18:19_

Closing in favour of #13633

---

_Closed by @AlexWaygood on 2024-10-04 18:19_

---

_Branch deleted on 2024-10-04 18:19_

---
