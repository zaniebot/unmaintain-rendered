```yaml
number: 12413
title: "[red-knot] add cycle-free while-loop control flow"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/while
created_at: 2024-07-19T23:31:50Z
updated_at: 2024-07-22T21:31:39Z
url: https://github.com/astral-sh/ruff/pull/12413
synced_at: 2026-01-12T15:55:41Z
```

# [red-knot] add cycle-free while-loop control flow

---

_@carljm_

Add support for while-loop control flow.

This doesn't yet include general support for terminals and reachability; that is wider than just while loops and belongs in its own PR.

This also doesn't yet add support for cyclic definitions in loops; that comes with enough of its own complexity in Salsa that I want to handle it separately.


---

_Review requested from @MichaReiser by @carljm on 2024-07-19 23:31_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-19 23:31_

---

_Comment by @github-actions[bot] on 2024-07-19 23:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @carljm on 2024-07-20 00:19_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:449 on 2024-07-20 10:08_

Nit: Some grouping of the statements could help readability
```suggestion
                self.visit_expr(&node.test);
                
                let pre_loop = self.flow_snapshot();
                let mut break_states: Vec<FlowSnapshot> = vec![];
                std::mem::swap(&mut self.loop_break_states, &mut break_states);
                self.visit_body(&node.body);
                std::mem::swap(&mut self.loop_break_states, &mut break_states);
                self.flow_merge(&pre_loop);
                
                self.visit_body(&node.orelse);
                for break_state in break_states {
                    self.flow_merge(&break_state);
                }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:431 on 2024-07-20 10:09_

```suggestion
                let mut break_states: Vec<FlowSnapshot> = std::mem::take(&mut self.loop_break_states);
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:449 on 2024-07-20 10:13_

The break state logic is a bit confusing. I think that's to some extend because we reuse the same name
```suggestion
								// Saved break states from any outer loop
                let mut saved_break_states = std::mem::take(&mut self.loop_break_states);
                self.visit_body(&node.body);
                let break_states = std::mem::replace(&mut self.loop_break_states, saved_break_states);
								for break_state in break_states {
                    self.flow_merge(&break_state);
                }

                self.flow_merge(&pre_loop);
                self.visit_body(&node.orelse);
```

I'm not sure if the ordering of the merges matters. If not, then I think having the merging of the break states (which are related to the for body) closer to `visit_body` helps understanding the flow.

---

_@MichaReiser approved on 2024-07-20 10:14_

Nice!

Would you mind extending our benchmarks fist to include a loop and then rebase this PR on top of it?

---

_@carljm reviewed on 2024-07-20 13:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:449 on 2024-07-20 13:47_

Ah, that is nicer, thanks!

The ordering of the merges does matter (relative to visiting the `else`). The `else` block cannot see the break states, because breaking out of a loop bypasses the `else` clause.

---

_Comment by @codspeed-hq[bot] on 2024-07-22 21:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/while)

### Merging #12413 will **improve performances by 4.26%**

<sub>Comparing <code>cjm/while</code> (e4813e8) with <code>main</code> (dbbe352)</sub>



### Summary

`⚡ 1` improvements
`✅ 32` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/while` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[numpy/globals.py]` | 30.4 µs | 29.2 µs | +4.26% |


---

_Merged by @carljm on 2024-07-22 21:27_

---

_Closed by @carljm on 2024-07-22 21:27_

---

_Branch deleted on 2024-07-22 21:27_

---
