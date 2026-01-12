```yaml
number: 18482
title: "[ty] AST garbage collection"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/ast-node-gc
created_at: 2025-06-05T17:54:11Z
updated_at: 2025-06-13T20:34:36Z
url: https://github.com/astral-sh/ruff/pull/18482
synced_at: 2026-01-12T15:56:19Z
```

# [ty] AST garbage collection

---

_@ibraheemdev_

## Summary

Garbage collect ASTs once we are done checking a given file. Queries with a cross-file dependency on the AST will reparse the file on demand. This reduces ty's peak memory usage by ~20-30%.

The primary change of this PR is adding a `node_index` field to every AST node, that is assigned by the parser. `ParsedModule` can use this to create a flat index of AST nodes any time the file is parsed (or reparsed). This allows `AstNodeRef` to simply index into the current instance of the `ParsedModule`, instead of storing a pointer directly.

The indices are somewhat hackily (using an atomic integer) assigned by the `parsed_module` query instead of by the parser directly. Assigning the indices in source-order in the (recursive) parser turns out to be difficult, and collecting the nodes during semantic indexing is impossible as `SemanticIndex` does not hold onto a specific `ParsedModuleRef`, which the pointers in the flat AST are tied to. This means that we have to do an extra AST traversal to assign and collect the nodes into a flat index, but the small performance impact (~3% on cold runs) seems worth it for the memory savings.

Part of https://github.com/astral-sh/ty/issues/214.

---

_Review requested from @carljm by @ibraheemdev on 2025-06-05 17:54_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-06-05 17:54_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-06-05 17:54_

---

_Review requested from @dcreager by @ibraheemdev on 2025-06-05 17:54_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-06-05 17:54_

---

_Review requested from @dhruvmanila by @ibraheemdev on 2025-06-05 17:54_

---

_Comment by @github-actions[bot] on 2025-06-05 18:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-06-05 19:21_

I'll review tomorrow. Can you add a log how many times we end up reparsing an ast and do a cold run on some project to see if we see any reparses to validate our assumption that this shouldn't happen?

---

_Comment by @codspeed-hq[bot] on 2025-06-05 19:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fast-node-gc)

### Merging #18482 will **degrade performances by 4.43%**

<sub>Comparing <code>ibraheem/ast-node-gc</code> (347934a) with <code>main</code> (76d9009)</sub>



### Summary

`⚡ 1` improvements  
`❌ 1` regressions  
`✅ 32` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fast-node-gc)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` linter/default-rules[numpy/globals.py] `` | 204.4 µs | 196.2 µs | +4.19% |
| ❌ | `` parser[unicode/pypinyin.py] `` | 306.6 µs | 320.8 µs | -4.43% |


---

_Comment by @ibraheemdev on 2025-06-05 22:08_

The incremental performance regression is expected. I'm not sure if we want less eager garbage collection for the LSP or if the initial hit is fine, and we instead try to keep recently reparsed ASTs around on subsequent runs? 

The cold check performance regression is pretty minor (~3% both on codspeed and locally on mypy_primer runs) even with the extra AST traversal.

---

_Comment by @carljm on 2025-06-05 23:16_

Thanks for working on this! I'm going to let @MichaReiser handle review :)

I'm curious about the new NodeIndex bit. Is the entire purpose of that so that we can use it in `AstNodeRef`, given we can no longer ensure a pointer into the AST will remain valid? Or are there other reasons we need it?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:83 on 2025-06-06 06:09_

It's not clear to me why this is the case, given that we collect ASTs mid-revision. 

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:87 on 2025-06-06 06:09_

Could we store the `NodeKind` and `TextRange` in debug builds and print them here. That would at least give us some idea of what node this is.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/node_key.rs`:5 on 2025-06-06 06:11_

Should we just store the `NodeIndex` here. That would also ensure that the type now shrinks to a `u32`. It probably even raises the question if `NodeKey` is still needed. It would probably be a good exercise to go through all uses and see where it can be replaced (maybe even with a `Vec` that directly indexes into the Nodes array)

---

_@MichaReiser reviewed on 2025-06-06 06:18_

This is great! 

I'd prefer to implement the ID assigning in the parser. Not only enables this other crates to make use of the index, but it also allows us to change `NodeIndex` to a `u32` to which is `Copy`. 

Do you think it would be possible to collect the nodes inside the parser (as part of the parsing step?). That would remove the need for an extra traversal. We could add an option to only collect them when requested.

Besides these code changes: It would be great to have some end to end benchmarks.

* Can you run `ty_benchmark` on some project and share the before/after runtime?
* Can you add some logging to when an AST gets reparsed because it was cleared before and run ty across some projects. Do we see any reparses within the same revisions? If so, why? 


For the LSP case. It probably makes sense for us not to purge any AST for a file that is in `project.open_files`. This should address any LSP concerns and I think even the perf regression in the benchmark



---

_@ibraheemdev reviewed on 2025-06-06 09:51_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/ast_node_ref.rs`:83 on 2025-06-06 09:51_

`ParsedModule` is an `Arc<ArcSwap<IndexedModule>>`. That outer `Arc` never changes, just like before. The garbage collection is invisible to Salsa.

---

_@ibraheemdev reviewed on 2025-06-06 09:52_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/node_key.rs`:5 on 2025-06-06 09:52_

Yeah I will do this once indices are assigned by the parser. Currently `NodeIndex` is `!Copy`.

---

_Comment by @ibraheemdev on 2025-06-06 09:58_

> I'm curious about the new NodeIndex bit. Is the entire purpose of that so that we can use it in AstNodeRef, given we can no longer ensure a pointer into the AST will remain valid? Or are there other reasons we need it?

`AstNodeRef` could still store a pointer to the AST, the difference is that it would have to store a weak pointer to the module (a strong reference would prevent garbage collection) and validate that reference before access. The problem comes when the module is garbage collected — we have to update the pointer to be into the new reparsed module. This has two problems:
- We need a way to mutate the `AstNodeRef`, which is tricky and probably requires it being a `Arc<Mutex<_>>` or similar.
- We have to traverse the entire AST to update every node that is accessed.

Creating a flat index of the AST upfront allow us to avoid both of those, and `AstNodeRef` becomes a simple index into the flat AST, which will remain stable even across garbage collection cycles within the same revision.

---

_Comment by @ibraheemdev on 2025-06-09 18:21_

The ty-benchmark results match the 3% regression on Codspeed.
```
black (cold)
Benchmark 1: /home/ibraheem/dev/astral/ruff/target/profiling/ty-new
  Time (mean ± σ):      56.9 ms ±   1.4 ms    [User: 290.4 ms, System: 74.9 ms]
  Range (min … max):    53.1 ms …  59.9 ms    51 runs
 
Benchmark 2: /home/ibraheem/dev/astral/ruff/target/profiling/ty-old
  Time (mean ± σ):      55.8 ms ±   1.4 ms    [User: 281.9 ms, System: 72.1 ms]
  Range (min … max):    52.1 ms …  59.0 ms    52 runs
 
Summary
  /home/ibraheem/dev/astral/ruff/target/profiling/ty-old ran
    1.02 ± 0.04 times faster than /home/ibraheem/dev/astral/ruff/target/profiling/ty-new

jinja (cold)
Benchmark 1: /home/ibraheem/dev/astral/ruff/target/profiling/ty-new
  Time (mean ± σ):      43.6 ms ±   1.6 ms    [User: 266.0 ms, System: 65.8 ms]
  Range (min … max):    40.7 ms …  47.5 ms    65 runs
 
 
Benchmark 2: /home/ibraheem/dev/astral/ruff/target/profiling/ty-old
  Time (mean ± σ):      41.7 ms ±   1.5 ms    [User: 259.7 ms, System: 62.9 ms]
  Range (min … max):    39.0 ms …  45.7 ms    74 runs
 
Summary
  /home/ibraheem/dev/astral/ruff/target/profiling/ty-old ran
    1.05 ± 0.05 times faster than /home/ibraheem/dev/astral/ruff/target/profiling/ty-new

isort (cold)
Benchmark 1: /home/ibraheem/dev/astral/ruff/target/profiling/ty-new
  Time (mean ± σ):      51.1 ms ±   2.5 ms    [User: 201.0 ms, System: 45.3 ms]
  Range (min … max):    46.1 ms …  57.0 ms    56 runs
 
 
Benchmark 2: /home/ibraheem/dev/astral/ruff/target/profiling/ty-old
  Time (mean ± σ):      49.5 ms ±   2.4 ms    [User: 197.4 ms, System: 45.2 ms]
  Range (min … max):    42.9 ms …  56.7 ms    62 runs

Summary
  /home/ibraheem/dev/astral/ruff/target/profiling/ty-old ran
    1.03 ± 0.07 times faster than /home/ibraheem/dev/astral/ruff/target/profiling/ty-new
  ```

---

_Review request for @carljm removed by @carljm on 2025-06-09 18:25_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/ast_node_ref.rs`:87 on 2025-06-09 18:27_

I updated this to just store the `ParsedModuleRef` in debug mode (which prevents garbage collection).

---

_@ibraheemdev reviewed on 2025-06-09 18:27_

---

_Comment by @ibraheemdev on 2025-06-09 18:32_

Given that the regression is quite minor I'm inclined to get this merged.

I spent some time trying to assign indices in the parser but it turned out to be non-trivial. The parser is recursive-descendant and we want to assign indices in source-order, so we run into problems where the parent node depends on the result of the recursion, so we don't know whether or not to claim an outer index before parsing the inner nodes. I think we would end up having to do mini-traversals to adjust the children indices — it's much easier to do this in a separate pass. If the parser also collected the nodes then it could do a simple pass over those and assign the indices then, but that could also be addressed in a future PR.

I also ran on a number of projects and was unable to trigger a reparse within a revision, I added some logs in case that ever happens.

---

_Comment by @github-actions[bot] on 2025-06-09 18:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-09 18:37_

---

_Comment by @MichaReiser on 2025-06-09 18:44_

> The parser is recursive-descendant and we want to assign indices in source-order, so we run into problems where the parent node depends on the result of the recursion, so we don't know whether or not to claim an outer index before parsing the inner nodes

Just trying to understand this better. Can you share a specific instance of this?

The main thing that I find slightly annoying is that `NodeId` isn't `Copy` because it's an `AtomicU32`. This is obviously not the end of the world but it would be nice if it could be avoided.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node_index.rs`:23 on 2025-06-09 18:47_

Could we change `NodeIndex` to be a wrapper around a `u32` and instead have an `AtomicNodeIndex` that internally uses an atomic? That would allow to use the non-atomic version in places where later mutations aren't necessary.

---

_@MichaReiser reviewed on 2025-06-09 18:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/node_index.rs`:72 on 2025-06-09 18:47_

What's the reason for not deriving those implementations? Is it because `AtomicU32` uses a stricter load ordering by default?

---

_@MichaReiser reviewed on 2025-06-09 18:47_

---

_@ibraheemdev reviewed on 2025-06-09 19:03_

---

_Review comment by @ibraheemdev on `crates/ruff_python_ast/src/node_index.rs`:72 on 2025-06-09 19:03_

`AtomicU32` doesn't implement of of those traits.

---

_Comment by @ibraheemdev on 2025-06-09 19:10_

> Just trying to understand this better. Can you share a specific instance of this?

In [`parse_expression_list`](https://github.com/astral-sh/ruff/blob/b44062b9ae08dcd8941ac76dbce2ab2c61840691/crates/ruff_python_parser/src/parser/expression.rs#L150) for example, we first parse the inner expression (which will increment the index), and then we conditionally wrap it in a `ExprTuple` node, which should have a lower index. Generally we would claim the parent index before recursing, but the conditional part makes this tricky.


---

_Comment by @carljm on 2025-06-09 19:20_

> the conditional part makes this tricky

Curious - would there be negative consequences to skipping an index in some scenarios?

---

_Comment by @ibraheemdev on 2025-06-09 19:50_

> Curious - would there be negative consequences to skipping an index in some scenarios?

Right now all indices are set strictly monotonic, so we can use a simple `Box<[Node]>` for the flat index, where each node's index matches its position in the list. If assigning indices in the parser means we have to switch to using a `HashMap`, or insert dummy nodes in the list, I'm not sure it's worth it.

---

_Comment by @MichaReiser on 2025-06-09 20:21_

Could we update the index when wrapping the node in a tuple (it may require a traversal of the inner node)

---

_Comment by @ibraheemdev on 2025-06-09 20:51_

We could, but I guess I don't really see the benefit of doing that. Collecting the nodes as part of parsing would have a similar problem I think (we'd have to insert parent nodes into the middle of the list before it's children). The separate pass is much simpler and doesn't seem to impact performance noticeably.

---

_Comment by @dhruvmanila on 2025-06-10 01:59_

Could we increase the index non-monotonically during parsing and then remove the gaps after the parsing is done? Or, would that be equivalent to visiting the parsed tree at the end to assign the index?

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-10 06:21_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:221 on 2025-06-10 07:22_

Would it be possible to implement the logic in `enter_node` instead of having to override each visit function? 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:215 on 2025-06-10 07:24_

It seems very unfortunate that the only reason that `NodeIndex` has to wrap an `AtomicU32` is because we don't have a `SourceOrderTransformer` (or `SourceOrderVisitorMut`). 

I think we should just add said visitor (as a separate PR and rebase on top of that). Most of the actual visiting logic is generated here https://github.com/astral-sh/ruff/blob/f6b7b7cfcca8112659e2e24a64521ce7df6651da/crates/ruff_python_ast/generate.py#L464-L482 and the rest is mostly boilerplate that you can copy from the `SourceOrderVisitor` implementation. 


---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:72 on 2025-06-10 07:27_

`info` level logging is reserved for messages that users need to be aware. We should use debug here.

```suggestion
                tracing::debug!(
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:73 on 2025-06-10 07:27_


```suggestion
                    "File `{}` was reparsed after being collected in the current Salsa revision.",
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/generate.py`:750 on 2025-06-10 07:32_

What this type is, isn't clear to me wrom it's name. First I thought it's about types that can be a "root" from what the parser returns (`ModModule` or `ModExpression`) but that's not what it is. Instead, it's a very specific selection of nodes? 

Reading through the code, I get the impression that it is related to nodes to which we assign IDs. Which brings up a related question: Why are we only assigning IDs to some nodes but not all? IMO, it's confusing that only some nodes have a `NodeId`. I'd prefer to just assign an ID to every node instead.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/expression.rs`:524 on 2025-06-10 07:38_

Rustc uses an explicit placeholder for "dummy" node IDs to make them easier recognizable. I think it could be helpful to do the same:

https://github.com/rust-lang/rust/blob/8a407a82848bbc926de1cbbbbcb381e1a96f5968/compiler/rustc_ast/src/node_id.rs#L23-L26

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:40 on 2025-06-10 07:44_

I'd prefer if debug and release builds wouldn't differ in such significant ways as when and how AST nodes get collected. I worry that it will make it very difficult to became aware of when we suddenly start reparsing modules very often (because we all use debug builds by default).

My preference here is to instead capture the `NodeKind` and `TextRange` of the node OR to capture the `Debug` output during creation and storing it in a `String`. I think the former is sufficient. It's rare that we directly debug print an `AstNodeRef` and it's always possible to call `node` to get the node and call debug on said node.

---

_Comment by @MichaReiser on 2025-06-10 07:44_

> I also ran on a number of projects and was unable to trigger a reparse within a revision, I added some logs in case that ever happens. 

Just double checking, did you do this check with a release build?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:36 on 2025-06-10 07:45_

Could we just use `File` here? Seems less magical and it allows us to remove the `ptr` method from `ParsedModule`.

---

_Review comment by @MichaReiser on `crates/ruff_db/src/parsed.rs`:163 on 2025-06-10 07:50_

Future: It could be interesting to collect some more numbers about the parsed module here. E.g. how many expressions are there? This could help to correctly initialize the `HashMaps` in the `SemanticIndexBuilder` that currently suffer from many resizes. 

---

_@MichaReiser reviewed on 2025-06-10 08:59_

I'm fine with the extra traversal for now. Eventough a 3% regression is fairly substantial for something that doesn't add new typing features. But the memory reduction is probably a good justification for it. 

But no longer doing it in the parser invalidates the main reason why we added the `NodeIndex` to every AST node. I did a quick search and it seems that the only place where we create `AstNodeRef`s is in the `SemanticIndexBuilder`. This makes me wonder if an approach closer to `AstNodeIds` would be a better fit where:

* `SemanticIndex` stores an: `nodes: IndexVec<NodeIndex, AnyRootNodeRef>` and an inverse map from `AnyNodeRef` (using ptr equality) to `NodeIndex`.
* `SemanticIndexBuilder` has a `register_node` method that, given a `node`, returns an `AstNodeRef` to it

This has a couple of advantages:

* No changes to the AST, no size increase of the AST
* We only register exactly the nodes we need and not all nodes. 
* It doesn't require an extra traversal

The obvious downside is that it requires an inverse hash map which might be coslty. What are your thoughts on building the index outside the AST? 


I wonder if it would make sense to split this PR into a few smaller PRs to, at least, get the most annoying changes landed sooner and also have a chance to better focus on some discussions:

* Add `NodeIndex`: Decide on which nodes have a `NodeIndex`, whether it should be an atomic or not
* Add a `SourceOrderMutVisitor` (if we decide to not use atomics)
* The actual AST GC implementation

But I leave this decision up to you. 

On the atomic discussion. I'm a bit undecided:

* I think we should remove the atomic and instead introduce a `SourceOrderMutVisitor` (or `SourceOrderTransformer`) if we don't plan on integrating the indexing/numbering into the semantic index step
* Otherwise, keeping atomic seems fine, but we should follow up with trying to integrate the pass into semantic indexing





---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:62 on 2025-06-10 10:12_

I think this no longer needs to be unsafe because dereferencing a node index that belongs to another module no longer results in UB but in a panic (out of bounds) or incorrect result. We could probably even add a debug assertion that verifies that `module_ref[index] == node`.

We should also update the comment

---

_@MichaReiser reviewed on 2025-06-10 10:12_

---

_Renamed from "AST garbage collection" to "[ty] AST garbage collection" by @MichaReiser on 2025-06-12 06:54_

---

_@ibraheemdev reviewed on 2025-06-12 20:06_

---

_Review comment by @ibraheemdev on `crates/ruff_db/src/parsed.rs`:221 on 2025-06-12 20:06_

The issue is that we need a `AnyRootNodeRef`, not a `AnyNodeRef`. The latter flattens nested enums, and `AstNodeRef` is often used with enum nodes, which we cannot recover from a reference to the inner value.

---

_@ibraheemdev reviewed on 2025-06-12 20:06_

---

_Review comment by @ibraheemdev on `crates/ruff_db/src/parsed.rs`:215 on 2025-06-12 20:06_

A mutable visitor doesn't work because we need to hold on to immutable references to the nodes to form the flat index while traversing the tree.

---

_@ibraheemdev reviewed on 2025-06-12 20:07_

---

_Review comment by @ibraheemdev on `crates/ruff_python_ast/generate.py`:750 on 2025-06-12 20:07_

I added some documentation to why we need a new type.. the name isn't great but I'm not sure of a better one. I believe all nodes have a `NodeId`, unless I missed one.

---

_@ibraheemdev reviewed on 2025-06-12 20:09_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/ast_node_ref.rs`:36 on 2025-06-12 20:09_

I don't think `File` is strong enough, because the `File` input ID will remain the same across revisions, while the `ParsedModule` will be re-created when the `File` changes in a new revision. We want to assert that the file has not changed, meaning that the AST will be identical across any `ParsedModuleRef` instance that belongs to the same `ParsedModule`.

---

_Comment by @ibraheemdev on 2025-06-12 20:15_

I explored a number of options here:
- Integrating the AST traversal into the semantic index. Unfortunately this does not work because the flat AST index is directly tied (through pointers) to a given `ParsedModuleRef`, while `SemanticIndex` does not hold on to a specific `ParsedModuleRef` instance. The `SemanticIndex` is transparent to the current instance of the AST, which is it now uses indices to reference nodes.
- Similarly, the `node_index` field being on the AST nodes directly is important. Storing a lookup map in `SemanticIndex` would be tricky for the reasons above — we don't want `SemanticIndex` to rely on any AST pointers, only indices.
- Removing the atomics from `NodeIndex` and adding a mutable visitor. This doesn't work because we hold on to immutable references to the nodes while traversing the tree, to form the flat index.

I think the current solution, where the flat index is created and managed directly by the `ParsedModule` and AST node indices are stored directly on the AST instance, makes the most sense and is easiest to reason about. The only future change I can see is if the node indices and collection are initialized even earlier by the parser, but I'm not sure that complexity is worth it (as I previously mentioned).

---

_Label `ty` added by @ibraheemdev on 2025-06-12 20:25_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__backspace_alias.snap`:4 on 2025-06-13 06:21_

I always forget, is this because the snapshots haven't been touched in a while and so `cargo-insta` has removed this field on a recent version or is it that you've an older version of `cargo-insta`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__backspace_alias.snap`:10 on 2025-06-13 06:23_

Should we generate a custom `Debug` implementation for the nodes to avoid including this field in the snapshot? Or, is it required that we include this field in the `Debug` implementation?

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/parsed.rs`:73 on 2025-06-13 06:26_

```suggestion
                    "File `{}` was reparsed after being collected in the current Salsa revision",
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:477 on 2025-06-13 06:33_

Can we remove the `unsafe` here too?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/definition.rs`:337 on 2025-06-13 06:34_

Can we remove the `unsafe` from this method, given that it's no longer UB if `node` doesn't belong to `parsed`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/generate.py`:780 on 2025-06-13 06:35_

We can add an example here to explain what does "root" mean like we can say that this will include the top level `Stmt` enum but will not include the individual variants of the `Stmt` enum.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:72 on 2025-06-13 06:36_

Nit: I still think it would be nice to have a non atomic `NodeIndex`, because it isn't really required here

---

_@dhruvmanila reviewed on 2025-06-13 06:36_

I'll leave this to Micha but a few things I noticed.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/ast_node_ref.rs`:112 on 2025-06-13 06:36_

Maybe: use `finish_non_exhaustive` to make clear that there's more but we can't show it

---

_@MichaReiser approved on 2025-06-13 06:38_

Nice! This is great and thank you for exploring all my not very fruitful alternatives. 

I think it could be nice to have a non atomic `NodeIndex` that we could use in `AstNodeRef`, `get_by_index` and in `NodeKey`. 

---

_Label `internal` added by @MichaReiser on 2025-06-13 06:38_

---

_@ibraheemdev reviewed on 2025-06-13 12:10_

---

_Review comment by @ibraheemdev on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__backspace_alias.snap`:4 on 2025-06-13 12:10_

I'm on the latest version of `cargo-insta` so I assume the former.

---

_@ibraheemdev reviewed on 2025-06-13 12:11_

---

_Review comment by @ibraheemdev on `crates/ruff_python_parser/src/snapshots/ruff_python_parser__string__tests__backspace_alias.snap`:10 on 2025-06-13 12:11_

I don't really see the benefit of removing this field, we may end up setting it in the parser later. I updated the debug implementation of `NodeIndex` to avoid the extra whitespace.

---

_Merged by @ibraheemdev on 2025-06-13 12:40_

---

_Closed by @ibraheemdev on 2025-06-13 12:40_

---

_Branch deleted on 2025-06-13 12:40_

---

_Comment by @carljm on 2025-06-13 20:34_

Just noticed this had been merged after I made the alpha-10 release. I think a 20-30% peak memory reduction is worth noting for users! So in future I think we could leave off the `internal` label on such a PR.

This is awesome!

---
