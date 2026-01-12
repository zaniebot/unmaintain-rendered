```yaml
number: 11670
title: "[red-knot] use reachable definitions in infer_expression_type"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/cfg2
created_at: 2024-06-01T01:59:03Z
updated_at: 2024-06-03T23:45:32Z
url: https://github.com/astral-sh/ruff/pull/11670
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] use reachable definitions in infer_expression_type

---

_@carljm_

## Summary

Switch name resolution in `infer_expression_type` from resolving the public type of a symbol, to resolving the reachable definitions of that symbol from the reference point, using the flow graph.

This surfaced a bug in the flow graph implementation and a bug in symbol table building, both of which are also fixed here.

The bug in flow graph implementation was that when we pushed and popped scopes, we didn't maintain a stack of "current flow nodes" in all stacked scopes, to be restored when we returned to that scope. Now we do.

The bug in symbol table building that we didn't visit the parts of functions and class definitions in the correct scopes. E.g. decorators should be visited in the outer scope, arguments should be visited inside the type-params scope (if any) but not inside the function body scope, and only the body itself should actually be visited inside the body scope. Fixing this requires that we no longer use `walk_stmt` here, instead we have to visit each individual component.

## Test Plan

Added test.


---

_Label `red-knot` added by @carljm on 2024-06-01 01:59_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-01 01:59_

---

_Review requested from @MichaReiser by @carljm on 2024-06-01 01:59_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:247 on 2024-06-01 08:18_

Nit: I think I would prefer a named struct here to awoid the awkward `.1` and `.0` which I always find hard to read. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:567 on 2024-06-01 08:18_

Nit: Maybe add an empty line in between to make it clear that the comment only applies to the assignment
```suggestion
                        self.flow_node_id = FlowGraph::start();
                        
                        return Some(def_node.definition.clone());
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:620 on 2024-06-01 08:20_

The compiler might be clever enough to work this out but maybe
```suggestion
        let (_, flow_node_id) = self.scopes.last_mut().expect("scope stack is never empty");
        *flow_node_id = new_flow_node_id;
    }
```

---

_@MichaReiser approved on 2024-06-01 08:22_

---

_@carljm reviewed on 2024-06-03 23:15_

---

_Review comment by @carljm on `crates/red_knot/src/symbols.rs`:567 on 2024-06-03 23:15_

Point taken, but this code changes in next PR anyway; not going to change it here which will just cause conflicts.

---

_Comment by @github-actions[bot] on 2024-06-03 23:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-06-03 23:45_

---

_Closed by @carljm on 2024-06-03 23:45_

---

_Branch deleted on 2024-06-03 23:45_

---
