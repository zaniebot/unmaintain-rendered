```yaml
number: 13075
title: "[red-knot] Add symbols for `for` loop variables"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-for-symbols
created_at: 2024-08-23T11:45:36Z
updated_at: 2024-08-23T22:40:29Z
url: https://github.com/astral-sh/ruff/pull/13075
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Add symbols for `for` loop variables

---

_Pull request opened by @AlexWaygood on 2024-08-23 11:45_

## Summary

This PR adds symbols introduced by `for` loops to red-knot:
- `x` in `for x in range(10): pass`
- `x` and `y` in `for x, y in d.items(): pass`
- `a`, `b`, `c` and `d` in `for [((a,), b), (c, d)] in foo: pass`

This is the first one of these PRs that I've done, so I hope I haven't missed anything!

## Test Plan

Several tests added, and the assertion in the benchmarks has been updated.

---

_Label `red-knot` added by @AlexWaygood on 2024-08-23 11:45_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-23 11:45_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-23 11:45_

---

_@AlexWaygood reviewed on 2024-08-23 11:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:863 on 2024-08-23 11:52_

Eh, I guess this is quite a naive approach to unpacked definitions, since things like this are also possible:

```pycon
>>> for x, *y in [(1, 2, 3), (4, 5, 6)]:
...     print(f"{x=}, {y=}")
... 
x=1, y=[2, 3]
x=4, y=[5, 6]
```

Happy to take this out for now if we don't want it, since I know that there are already some TODOs in this file about unpacked assignments (and https://github.com/astral-sh/ruff/issues/12698 is open)

---

_Comment by @codspeed-hq[bot] on 2024-08-23 11:56_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/redknot-for-symbols)

### Merging #13075 will **improve performances by 9.06%**

<sub>Comparing <code>alex/redknot-for-symbols</code> (3fc3db2) with <code>main</code> (99df859)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `alex/redknot-for-symbols` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 904.1 µs | 829 µs | +9.06% |


---

_Comment by @github-actions[bot] on 2024-08-23 12:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:596 on 2024-08-23 12:17_

Nit: I would move the assertion closer to where we want to ensure that the assignment is null

```suggestion
                self.add_standalone_expression(iter);
                self.visit_expr(iter);
                debug_assert!(self.current_assignment.is_none());
                self.current_assignment = Some(for_stmt.into());
```

Unrelated to this PR: We could do something more fancy if this is an assertion that we want whenever we set an assertion by:

* Adding a `current_assignment(for_stmt.into()) -> AssignmentGuard` method. It asserts that `current_assignment` is `None`
* The `AssignmentGuard` is a wrapper around a [`DropBomb`](https://docs.rs/drop_bomb/latest/drop_bomb/)
* Add a `clear_assignment(guard)` method (or guard.clear(&mut self)`)  that unsets the assignment and defuses the bomb.

---

_@MichaReiser reviewed on 2024-08-23 12:18_

---

_@AlexWaygood reviewed on 2024-08-23 12:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:596 on 2024-08-23 12:20_

That sounds fun to explore!

---

_@dhruvmanila reviewed on 2024-08-23 12:45_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:596 on 2024-08-23 12:45_

This existed for a while in the new parser, you can refer to it:

https://github.com/astral-sh/ruff/blob/1475a4d2a8a487a48593a53be33b9f0fac1d1a55/crates/ruff_python_parser/src/parser/mod.rs#L232-L248

Instead of `flags`, it would be `current_assignment: CurrentAssignment`

---

_@dhruvmanila reviewed on 2024-08-23 12:50_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:42 on 2024-08-23 12:50_

I think we would need to define a custom struct which determines which variable this definition is for. Considering the below example:
```py
for k, v in items:
	...
```
We will need to create two definitions for `k` and `v` variable and this new struct would capture that. It should probably be similar to `AssignmentDefinitionNodeRef` (similarly `AssignmentDefinitionNodeKind`), so:
```rs
pub(crate) struct ForStmtDefinitionNodeRef<'a> {
	pub(crate) node: &'a ast::StmtFor,
	pub(crate) target: &'a ast::ExprName,
}
```

Maybe we could get away by just storing the `iter` instead of the entire node because the variable type can be inferred just from that expression.

---

_@dhruvmanila reviewed on 2024-08-23 12:52_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 12:52_

I think this should create two definitions each for `x` and `y` variable.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 13:18_

So I'm currently creating two definitions: one for `x` and one for `y`. Is that what you mean? It's possible I'm not doing that in the "right place", though -- I'm doing it in `add_maybe_multiple_for_definitions()` in `infer.rs`.

I might also be misunderstanding what you mean, as this is my first PR adding new symbols to red-knot

---

_@AlexWaygood reviewed on 2024-08-23 13:18_

---

_@dhruvmanila reviewed on 2024-08-23 13:57_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 13:57_

Ah, I see. Sorry for not noticing that earlier. 

> So I'm currently creating two definitions: one for `x` and one for `y`. Is that what you mean? It's possible I'm not doing that in the "right place", though -- I'm doing it in `add_maybe_multiple_for_definitions()` in `infer.rs`.

I think what's happening here is that the _types_ are being defined for two definitions but there's still only a single definition. The reason being that the definitions are added at the symbol level and are defined by a unique key (`NodeKey` - a cheaper alternative to the node to reduce the cost of hashing). These are defined in the semantic index builder as is done by `add_definition` call. So, as the definition contains the entire `for` statement, the key would still be the same for each symbol in the target expression. In the example, for both `x` and `y` the definition node key is still the same for statement. 

Each symbol should have a unique definition which, in one way, helps the inference engine to determine the type of that symbol. I've a suggestion in my other comment (https://github.com/astral-sh/ruff/pull/13075/files#r1728920074) on what could that look like.

https://github.com/astral-sh/ruff/blob/342473fd3f1ce40958719910f17154770b641ebd/crates/red_knot_python_semantic/src/semantic_index/builder.rs#L191-L192

Does this help?

---

_@AlexWaygood reviewed on 2024-08-23 15:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 15:42_

Thanks, that helps a lot! I see the error now. I'm sort-of surprised that this didn't cause any test failures or panics. (Is there another test I should have added, that would have caught this? Or is it not worth it to worry about that?)

---

_@dhruvmanila reviewed on 2024-08-23 16:08_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 16:08_

> (Is there another test I should have added, that would have caught this? Or is it not worth it to worry about that?)

That's a good question. I don't know if there's any way to test that _specific_ behavior so I wouldn't worry about it right now. This would be something to keep in mind when designing the test infrastructure where we assert that each symbol has a unique definition key or something along that line.

---

_Converted to draft by @AlexWaygood on 2024-08-23 16:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:600 on 2024-08-23 16:11_

We could add a TODO reminder here about needing control flow still

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:634 on 2024-08-23 16:13_

Here's where we'll need to be passing in some node that is specific to the symbol (i.e. the name node), in addition to the overall for-stmt node, more like we do for Assign above (another case where unpacking can happen), so as to distinguish what the Definition is actually for.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:863 on 2024-08-23 16:16_

Yeah I think we should probably remove this and follow the model of `infer_assignment_statement`, leaving unpacking as a TODO for now; we can tackle unpacking as a single project across all the definition kinds where it can occur.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:880 on 2024-08-23 16:18_

This will need to become a lookup by a specific name node, rather than the overall for statement node, so that we can lookup the right definition for each symbol. For now we can just match on the target and lookup the definition by it if its a name node, and leave unpacking as a TODO.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 16:18_

I think this is mostly a consequence of doing Definitions without the actual type inference; if you'd had tests for the actual type inference of unpacked names, this would have failed in tests. (I'm not suggesting you should have done anything differently in that regard; that's how we've been splitting up these PRs in general, maybe it wasn't the best choice.)

---

_@carljm requested changes on 2024-08-23 16:18_

---

_@carljm reviewed on 2024-08-23 16:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 16:23_

Oh, another test assertion that I think would have caught this would be to assert that the two definitions you get back for the two symbols are actually different definition IDs? But obviously it's not likely you'd write that assertion without already being aware of the issue. 

---

_@carljm reviewed on 2024-08-23 17:19_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 17:19_

Sorry for triple-posting here :) It just occurred to me that another easy thing we could do that would have caught this up-front (and you could add in this PR) would be to `debug_assert` on the return value of `self.definitions_by_node.insert(...)` inside `add_definition`; I think if we are ever overwriting a previously-inserted `Definition` there, it's a bug.

---

_@AlexWaygood reviewed on 2024-08-23 18:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:908 on 2024-08-23 18:20_

I think this is probably not really the right place for this TODO? Since the inferring of the iterator type (and the unpacking of whatever is returned from the iterator's `__next__` method) really needs to be done at a higher level? but I'm also not really sure where else to put this TODO?

---

_@AlexWaygood reviewed on 2024-08-23 18:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 18:24_

Hmm, I tried adding this debug assertion:

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder.rs b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
index 9805d2501..4185765ba 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -190,8 +190,13 @@ impl<'db> SemanticIndexBuilder<'db> {
             countme::Count::default(),
         );
 
-        self.definitions_by_node
+        let definition_already_existed = self
+            .definitions_by_node
             .insert(definition_node.key(), definition);
+        debug_assert_eq!(
+            definition_already_existed, None,
+            "Should never overwrite an existing definition!"
+        );
         self.current_use_def_map_mut()
             .record_definition(symbol, definition);
```

But it causes a panic in the corpus test (and not due to this PR, I don't think?)

```
checking "/Users/alexw/dev/ruff/crates/red_knot_workspace/resources/test/corpus/58_async_for_dict_comp.py"
thread 'corpus_no_panic' panicked at crates/red_knot_python_semantic/src/semantic_index/builder.rs:196:9:
assertion `left == right` failed: Should never overwrite an existing definition!
  left: Some(Definition { [salsa id]: Id(45d1), file: File { [salsa id]: Id(cc3), path: System("/Users/alexw/dev/ruff/crates/red_knot_workspace/resources/test/corpus/58_async_for_dict_comp.py"), permissions: Some(33188), revision: FileRevision(31781594018582998360096355038), status: Exists, count: Count { ghost: PhantomData<fn(ruff_db::files::File)> } }, file_scope: FileScopeId(2), symbol: ScopedSymbolId(0), node: Comprehension(ComprehensionDefinitionKind { node: AstNodeRef(Comprehension { range: 31..54, target: Tuple(ExprTuple { range: 41..45, elts: [Name(ExprName { range: 41..42, id: Name("k"), ctx: Store }), Name(ExprName { range: 44..45, id: Name("v"), ctx: Store })], ctx: Store, parenthesized: false }), iter: Call(ExprCall { range: 49..54, func: Name(ExprName { range: 49..52, id: Name("gen"), ctx: Load }), arguments: Arguments { range: 52..54, args: [], keywords: [] } }), ifs: [], is_async: true }), first: true }), count: Count { ghost: PhantomData<fn(red_knot_python_semantic::semantic_index::definition::Definition)> } })
 right: None
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    corpus_no_panic
```

So I think I'll leave this for now?

---

_Marked ready for review by @AlexWaygood on 2024-08-23 18:25_

---

_Comment by @AlexWaygood on 2024-08-23 18:26_

Thanks @dhruvmanila and @carljm! Pushed some changes. Lmk if it's still not quite right.

---

_Review requested from @carljm by @AlexWaygood on 2024-08-23 18:26_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-08-23 18:26_

---

_@carljm reviewed on 2024-08-23 22:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:1117 on 2024-08-23 22:01_

Yeah, makes sense; if we're already not keeping this invariant, it's out of scope for this PR. I created an issue to follow up: https://github.com/astral-sh/ruff/issues/13085

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:908 on 2024-08-23 22:23_

In the current architecture, this is the right place for the TODO; we will have to repeat this logic for each definition, in the case of unpacking. We don't currently have another layer available at which to cache it. Caching also has an overhead and sometimes it's better to just repeat some work. But we can also re-evaluate this when we implement unpacking.

---

_@carljm approved on 2024-08-23 22:24_

Looks great, thank you!!

---

_@AlexWaygood reviewed on 2024-08-23 22:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:908 on 2024-08-23 22:39_

I'm not 100% sure we're on the same page here (but this may also just be me still getting used to this architecture) — anyway, I don't think it's massively important where this TODO lives, so I'll leave this for now!

---

_Merged by @AlexWaygood on 2024-08-23 22:40_

---

_Closed by @AlexWaygood on 2024-08-23 22:40_

---

_Branch deleted on 2024-08-23 22:40_

---
