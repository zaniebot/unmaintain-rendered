```yaml
number: 19025
title: "[ty] Use an interval map for scopes by expression"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/scopes-by-expression-interval-map
created_at: 2025-06-29T14:45:09Z
updated_at: 2025-07-14T11:51:00Z
url: https://github.com/astral-sh/ruff/pull/19025
synced_at: 2026-01-12T15:56:30Z
```

# [ty] Use an interval map for scopes by expression

---

_@MichaReiser_

## Summary

The semantic index stores a map from expression to scope because we need to know in which `TypeInference` (scope) to look up the expression's type. Today, we use a hash map to store the expression-to-scope mapping. 

This PR replaces the hash map with an interval map (vector-based) that maps a range of node IDs (expressions) to their corresponding scope. The advantage of an interval map over a hash map is that it reduces memory consumption from `O(expressions)` to `O(~scopes)`. 

The main downside (other than increased complexity) is that the lookup complexity increases from `O(1)` to `O(log(~scopes))`. Looking at the benchmark results, the fact that we need to write less data outweighs the slightly slower lookup times. 

The instrumented benchmarks show a 1-2% performance improvement. I measured memory consumption on a large project and the overall memory consumption of all semantic indices decreased by about 5%,

## Test plan

`cargo test`

---

_Label `internal` added by @MichaReiser on 2025-06-29 14:45_

---

_Label `ty` added by @MichaReiser on 2025-06-29 14:45_

---

_Label `internal` added by @MichaReiser on 2025-06-29 14:45_

---

_Label `ty` added by @MichaReiser on 2025-06-29 14:45_

---

_Comment by @github-actions[bot] on 2025-06-29 14:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~66MB
+     memo fields = ~63MB

prefect (https://github.com/PrefectHQ/prefect)
-     memo fields = ~568MB
+     memo fields = ~541MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-06-29 14:55_

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

_Marked ready for review by @MichaReiser on 2025-07-12 16:26_

---

_Review requested from @carljm by @MichaReiser on 2025-07-12 16:26_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-12 16:26_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-12 16:26_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-12 16:26_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2661 on 2025-07-14 07:28_

Is this something that would change with the proposal in https://github.com/astral-sh/ruff/pull/19271?

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2696 on 2025-07-14 07:42_

Using `rposition` may require walking large distances back if the inner interleaving scopes are large? Would it make sense to use a binary search here? Something like
```suggestion
        if self
            .expression_and_scope
            .last()
            .is_none_or(|last| last.0 <= expression_index)
        {
            self.expression_and_scope.push((expression_index, scope));
            return;
        }

        if self.expression_and_scope[0].0 > expression_index {
            self.expression_and_scope
                .insert(0, (expression_index, scope));
            return;
        }

        let insertion_point = self
            .expression_and_scope
            .partition_point(|(index, _)| *index <= expression_index);
```

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2722 on 2025-07-14 07:51_

Not sure if better, but avoids wrapping/unwrapping of node indices into u32's (with theoretical overflow problems)
```suggestion
        let mut range = first.0..=first.0;

        for (index, scope) in iter {
            if scope == current_scope {
                range = (*range.start())..=index;
                continue;
            }

            interval_map.push((range, current_scope));

            current_scope = scope;
            range = index..=index;
```

---

_@sharkdp approved on 2025-07-14 07:51_

Very nice — thank you!

---

_@MichaReiser reviewed on 2025-07-14 07:53_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2661 on 2025-07-14 07:53_

Yes, but I don't think it would invalidate the entire approach. Instead, we would have to use a regular `sort` call in `build` before building the interval map (and Rust's sorting claims to be pretty good at sorting mostly sorted data)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:2696 on 2025-07-14 07:54_

I initially used a binary search and it was slower :) 

I think this is simply because we don't have any large interleaved scopes. The only weird scoping is with comprehensions and they are only very few expressions

---

_@MichaReiser reviewed on 2025-07-14 07:54_

---

_Merged by @MichaReiser on 2025-07-14 11:50_

---

_Closed by @MichaReiser on 2025-07-14 11:50_

---

_Branch deleted on 2025-07-14 11:51_

---
