```yaml
number: 11673
title: "[red-knot] add if-statement support to FlowGraph"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/cfg4
created_at: 2024-06-01T04:01:16Z
updated_at: 2024-06-05T22:03:42Z
url: https://github.com/astral-sh/ruff/pull/11673
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] add if-statement support to FlowGraph

---

_@carljm_

## Summary

Add if-statement support to FlowGraph. This introduces branches and joins in the graph for the first time.

## Test Plan

Added tests.

---

_Label `red-knot` added by @carljm on 2024-06-01 04:01_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-01 04:01_

---

_Review requested from @MichaReiser by @carljm on 2024-06-01 04:01_

---

_@plredmond reviewed on 2024-06-01 06:56_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:570 on 2024-06-01 06:56_

Does the order that we explore the basic blocks prior to a phi node matter? I guess it's going to just be whatever order they are within `.predecessors`

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:574 on 2024-06-01 08:00_

Nit: You could have kept the loop

```rust

        loop {
						let flow_node_id = self.pending.pop()?;
            match &self.table.flow_graph.flow_nodes_by_id[flow_node_id] {
                FlowNode::Start => break Some(Definition::None),
                FlowNode::Definition(def_node) => {
                    if def_node.symbol_id == self.symbol_id {
                        break Some(def_node.definition.clone());
                    }
                    self.pending.push(def_node.predecessor);
                }
                FlowNode::Branch(branch_node) => {
                    self.pending.push(branch_node.predecessor);
                }
                FlowNode::Phi(phi_node) => {
                    self.pending.extend_from_slice(&phi_node.predecessors);
                }
            }
        }
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:608 on 2024-06-01 08:06_

I'm not sure how the `unreachable` PR solved this but having a `Vec` here is somewhat expensive because we now have to allocate whenever two branches join, and that's very common. 

It might be worth to spend some time exploring how we can improve this by:

* Change the `Phi` node to only ever join 2 branches, and chain them together
* Store the predecessors in a centralized storage and only store the `Range` in the `PhiFlowNode` into that `Vec` (requires that we can write all predecessor entries at once). 
* I would prefer not to, but these are valid options:
  * Use an arena allocator for the `Vec`s 
  * Use `SmallVec` (reduces the cost for reading but we still pay the cost of having to call `Drop` when the node goes out of Scope and it adds a cost in the read path which I often found too expensive)


---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:914 on 2024-06-01 08:09_

This is a nit, but I think adding some empty lines to group the logic would help readers to better understand what's happening. It now reads like a text without any paragraphs, which makes it hard to scan.
```suggestion
                self.visit_expr(&node.test);

                let pre_body = self.current_flow_node();
                let if_branch = self.new_flow_node(FlowNode::Branch(BranchFlowNode {
                    predecessor: pre_body,
                }));
                
                self.set_current_flow_node(if_branch);
                self.visit_body(&node.body);
                
                let mut phi_node = PhiFlowNode {
                    predecessors: vec![self.current_flow_node()],
                };
                let mut last_branch = if_branch;
                let mut last_branch_is_else = false;
                
                for clause in &node.elif_else_clauses {
                    if clause.test.is_some() {
                        let clause_branch = self.new_flow_node(FlowNode::Branch(BranchFlowNode {
                            predecessor: last_branch,
                        }));
                        last_branch = clause_branch;
                        self.set_current_flow_node(clause_branch);
                    } else {
                        self.set_current_flow_node(last_branch);
                        last_branch_is_else = true;
                    }
                    self.visit_elif_else_clause(clause);
                    phi_node.predecessors.push(self.current_flow_node());
                }
                
                if !last_branch_is_else {
                    phi_node.predecessors.push(last_branch);
                }
                
                let phi_node_id = self.new_flow_node(FlowNode::Phi(phi_node));
                self.set_current_flow_node(phi_node_id);
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:898 on 2024-06-01 08:10_

Unrelated question. Does our CFG support branching inside expression or only on a statement level?

---

_@MichaReiser approved on 2024-06-01 08:15_

Nice! The functionality looks good to me. You may want to consider to move the implementation to `symbols/cfg` (or rename `symbols` to `semantic` or `binding`). 

I think we should spend some more time to see if we can come up with a design that avoids the nested `Vec` inside of the `Phi` node. The `Vec` is problematic because it means that we create a ton of allocations because Phi nodes are very common. 

---

_@carljm reviewed on 2024-06-01 14:22_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:574 on 2024-06-01 14:22_

Good point. Do you find this clearer or more readable than the `while let`?

---

_@carljm reviewed on 2024-06-01 14:24_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:898 on 2024-06-01 14:24_

It should support within expressions too, but it's a good callout that I should prioritize implementing a case of that to surface any issues. Also walrus expressions (within expression definitions). 

---

_@MichaReiser reviewed on 2024-06-01 14:25_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:574 on 2024-06-01 14:25_

Not really. I may have a slight preference to `loop` but not that it matters. I was mainly wondering if you rewrote it to a `while` loop because it wasn't clear to you how to achieve the same with a loop. 

---

_Comment by @carljm on 2024-06-01 14:26_

I was holding off on renaming / restructuring for now so as to minimize conflicts with your salsa work and Patrick's symbol table work. But I definitely want to do that. 

---

_@carljm reviewed on 2024-06-01 15:55_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:570 on 2024-06-01 15:55_

In principle it shouldn't matter; in practice it may be more intuitive if, when we synthesize a union type from multiple reachable definitions, we try to display that union with elements in source order. I can look at this.

---

_@plredmond reviewed on 2024-06-03 04:11_

---

_Review comment by @plredmond on `crates/red_knot/src/symbols.rs`:570 on 2024-06-03 04:11_

I wonder if the text-locations on the AST nodes can be included in these predecessors, and then instead of a vec we could store them as a heap. That would allow you to add them in any order but obtain source order.

---

_Comment by @MichaReiser on 2024-06-03 06:46_

@carljm I'm still very far off with my salsa work and I probably need to redo my stack once we figured out the entire data model (or at least parts of it) because  @AlexWaygood already made many changes that I need to incorporate somehow. So, don't feel blocked by my salsa work but I'm sure Patrick appreciates it. 

---

_@carljm reviewed on 2024-06-03 23:44_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:570 on 2024-06-03 23:44_

I think we could do that, but we can also achieve the same result just by being careful about the order we list predecessors in a phi node and push them to the pending list in the flow graph.

---

_Comment by @github-actions[bot] on 2024-06-04 00:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/selection_histogram.py#L88'>examples/server/app/selection_histogram.py:88:42:</a> NPY001 [*] Type alias `np.bool` is deprecated, replace with builtin type
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/server/app/selection_histogram.py#L88'>examples/server/app/selection_histogram.py:88:42:</a> NPY001 [*] Type alias `np.bool` is deprecated, replace with builtin type
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| NPY001 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_@carljm reviewed on 2024-06-04 18:44_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:570 on 2024-06-04 18:44_

Got this working and pushed.

---

_@carljm reviewed on 2024-06-04 18:45_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:608 on 2024-06-04 18:45_

Switched to exactly two predecessors per Phi node, no more vec. Thanks for the suggestion!

---

_@carljm reviewed on 2024-06-04 18:46_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:574 on 2024-06-04 18:46_

Switched back to the loop.

---

_@carljm reviewed on 2024-06-04 19:21_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:914 on 2024-06-04 19:21_

Good call; I did this, and also added comments explaining the algorithm.

---

_Comment by @carljm on 2024-06-04 19:36_

Refactoring CFG out of symbol table is tracked in https://github.com/astral-sh/ruff/issues/11657, not going to do it in this PR.

---

_Merged by @carljm on 2024-06-04 21:09_

---

_Closed by @carljm on 2024-06-04 21:09_

---

_Branch deleted on 2024-06-04 21:09_

---

_@carljm reviewed on 2024-06-05 22:03_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:570 on 2024-06-05 22:03_

I was wrong here (inadequate test cases.) It's not generally possible to do this the way I was trying to do it (statically in the structure of the flow graph), since some names may be defined in branches where others aren't. If we really want to guarantee source order of inferred unions, we would have to explicitly include source-order information in definitions. Not going to prioritize that right now, though.

---
