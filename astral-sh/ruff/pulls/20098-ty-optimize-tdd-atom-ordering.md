```yaml
number: 20098
title: "[ty] Optimize TDD atom ordering"
type: pull_request
state: merged
author: sharkdp
labels:
  - performance
  - great writeup
  - ty
assignees: []
merged: true
base: main
head: david/fix-1091
created_at: 2025-08-26T15:32:29Z
updated_at: 2025-08-28T00:45:28Z
url: https://github.com/astral-sh/ruff/pull/20098
synced_at: 2026-01-12T15:56:54Z
```

# [ty] Optimize TDD atom ordering

---

_@sharkdp_

## Summary

While looking at some logging output that I added to `ReachabilityConstraintBuilder::add_and_constraint` in order to debug https://github.com/astral-sh/ty/issues/1091, I noticed that it seemed to suggest that the TDD was built in an imbalanced way for code like the following, where we have a sequence of non-nested `if` conditions:

```py
def f(t1, t2, t3, t4, …):
    x = 0
    if t1:
        x = 1
    if t2:
        x = 2
    if t3:
        x = 3
    if t4:
        x = 4
    …
```

To understand this a bit better, I added some code to the `ReachabilityConstraintBuilder` to render the resulting TDD. On `main`, we get a tree that looks like the following, where you can see a pattern of N sub-trees that grow linearly with N (number of `if` statements). This results in an overall tree structure that has N² nodes (see graph below):

<img alt="normal order" src="https://github.com/user-attachments/assets/aab40ce9-e82a-4fcd-823a-811f05f15f66" />

If we zoom in to one of these subgraphs, we can see what the problem is. When we add new constraints that represent combinations like `t1 AND ~t2 AND ~t3 AND t4 AND …`, they start with the evaluation of "early" conditions (`t1`, `t2`, …). This means that we have to create new subgraphs for each new `if` condition because there is little sharing with the previous structure. We evaluate the Boolean condition in a right-associative way: `t1 AND (~t2 AND (~t3 AND t4)))`:

<img width="500" align="center" src="https://github.com/user-attachments/assets/31ea7182-9e00-4975-83df-d980464f545d" />

If we change the ordering of TDD atoms, we can change that to a left-associative evaluation: `(((t1 AND ~t2) AND ~t3) AND t4) …`. This means that we can re-use previous subgraphs `(t1 AND ~t2)`, which results in a much more compact graph structure overall (note how "late" conditions are now at the top, and "early" conditions are further down in the graph):

<img alt="reverse order" src="https://github.com/user-attachments/assets/96a6b7c1-3d35-4192-a917-0b2d24c6b144" />

If we count the number of TDD nodes for a growing number if `if` statements, we can see that this change results in a slower growth. It's worth noting that the growth is still superlinear, though:

<img width="800" height="600" alt="plot" src="https://github.com/user-attachments/assets/22e8394f-e74e-4a9e-9687-0d41f94f2303" />

On the actual code from the referenced ticket (the `t_main.py` file reduced to its main function, with the main function limited to 2000 lines instead of 11000 to allow the version on `main` to run to completion), the effect is much more dramatic. Instead of 26 million TDD nodes (`main`), we now only create 250 thousand (this branch), which is slightly less than 1%.

The change in this PR allows us to build the semantic index and type-check the problematic `t_main.py` file in https://github.com/astral-sh/ty/issues/1091 in 9 seconds. This is still not great, but an obvious improvement compared to running out of memory after *minutes* of execution.

An open question remains whether this change is beneficial for all kinds of code patterns, or just this linear sequence of `if` statements. It does not seem unreasonable to think that referring to "earlier" conditions is generally a good idea, but I learned from Doug that it's generally not possible to find a TDD-construction heuristic that is non-pathological for all kinds of inputs. Fortunately, it seems like this change here results in performance improvements across *all of our benchmarks*, which should increase the confidence in this change:

| Benchmark           | Improvement |
|---------------------|-------------------------|
| hydra-zen           | +13%                    |
| DateType            | +5%                     |
| sympy (walltime)    | +4%                     |
| attrs               | +4%                     |
| pydantic (walltime) | +2%                     |
| pandas (walltime)   | +2%                     |
| altair (walltime)   | +2%                     |
| static-frame        | +2%                     |
| anyio               | +1%                     |
| freqtrade           | +1%                     |
| colour-science      | +1%                     |
| tanjun              | +1%                     |

closes https://github.com/astral-sh/ty/issues/1091


---

_Label `ty` added by @sharkdp on 2025-08-26 15:32_

---

_Comment by @github-actions[bot] on 2025-08-26 15:35_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-26 15:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~273MB
+ TOTAL MEMORY USAGE: ~260MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-08-26 15:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Ffix-1091?runnerMode=Instrumentation)

### Merging #20098 will **improve performances by 13.81%**

<sub>Comparing <code>david/fix-1091</code> (8e988ae) with <code>main</code> (5663426)</sub>



### Summary

`⚡ 2` improvements  
`✅ 40` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` attrs `` | 375.1 ms | 360.4 ms | +4.09% |
| ⚡ | `` hydra-zen `` | 803.6 ms | 706.1 ms | +13.81% |


---

_Renamed from "[ty] Reverse TDD node ordering" to "[ty] Optimize TDD atom ordering" by @sharkdp on 2025-08-27 09:52_

---

_Marked ready for review by @sharkdp on 2025-08-27 10:16_

---

_Review requested from @carljm by @sharkdp on 2025-08-27 10:16_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-27 10:16_

---

_Review requested from @dcreager by @sharkdp on 2025-08-27 10:16_

---

_Label `performance` added by @AlexWaygood on 2025-08-27 10:19_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:429 on 2025-08-27 14:29_

I think this comment is now quite confusing.

Would it be equivalent here if we compared interiors in the "forward" direction, swapped the `Greater` and `Less` cases below, and then reversed the direction in which we use this ordering? It seems like that would make it possible to describe the behavior of this method more straightforwardly.

---

_@carljm approved on 2025-08-27 14:30_

Awesome investigation and results!

---

_@sharkdp reviewed on 2025-08-27 15:34_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:429 on 2025-08-27 15:34_

The problem is that the terminal nodes need to be larger than anything else. If we swap the `Greater` and `Less` cases below instead, that would also invert the logic for terminal nodes (which violates invariants and leads to panics). But maybe I haven't understood what you meant by this:

> and then reversed the direction in which we use this ordering

---

_@carljm reviewed on 2025-08-27 15:49_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:429 on 2025-08-27 15:49_

What I meant was that in each of the two places we call `.cmp_atoms` we would also swap the match arms for `Greater` and `Less`, so the invariant becomes that interior nodes can only have edges to "smaller" nodes and terminals are considered "smaller" than any internal node. Then I think we get the same effect but eliminate the confusing idea that a "larger" node has a "smaller" atom.

This change passes mdtests for me locally:

```diff
diff --git a/crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs b/crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs
index 9042e919be..2545413f0e 100644
--- a/crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs
+++ b/crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs
@@ -435,9 +435,9 @@ impl ReachabilityConstraintsBuilder {
         if a == b || (a.is_terminal() && b.is_terminal()) {
             Ordering::Equal
         } else if a.is_terminal() {
-            Ordering::Greater
-        } else if b.is_terminal() {
             Ordering::Less
+        } else if b.is_terminal() {
+            Ordering::Greater
         } else {
             self.interiors[a].atom.cmp(&self.interiors[b].atom)
         }
@@ -549,7 +549,7 @@ impl ReachabilityConstraintsBuilder {
                 };
                 (a_node.atom, if_true, if_ambiguous, if_false)
             }
-            Ordering::Less => {
+            Ordering::Greater => {
                 let a_node = self.interiors[a];
                 let if_true = self.add_or_constraint(a_node.if_true, b);
                 let if_false = self.add_or_constraint(a_node.if_false, b);
@@ -560,7 +560,7 @@ impl ReachabilityConstraintsBuilder {
                 };
                 (a_node.atom, if_true, if_ambiguous, if_false)
             }
-            Ordering::Greater => {
+            Ordering::Less => {
                 let b_node = self.interiors[b];
                 let if_true = self.add_or_constraint(a, b_node.if_true);
                 let if_false = self.add_or_constraint(a, b_node.if_false);
@@ -615,7 +615,7 @@ impl ReachabilityConstraintsBuilder {
                 };
                 (a_node.atom, if_true, if_ambiguous, if_false)
             }
-            Ordering::Less => {
+            Ordering::Greater => {
                 let a_node = self.interiors[a];
                 let if_true = self.add_and_constraint(a_node.if_true, b);
                 let if_false = self.add_and_constraint(a_node.if_false, b);
@@ -626,7 +626,7 @@ impl ReachabilityConstraintsBuilder {
                 };
                 (a_node.atom, if_true, if_ambiguous, if_false)
             }
-            Ordering::Greater => {
+            Ordering::Less => {
                 let b_node = self.interiors[b];
                 let if_true = self.add_and_constraint(a, b_node.if_true);
```

---

_@carljm reviewed on 2025-08-27 15:52_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:429 on 2025-08-27 15:52_

But I don't think this is worth a lot of bother. If there's an issue with that code change and we need to leave the code the same, and accept that "smaller atom" means "larger node", then I think there's still something confusing/wrong that we should fix in the docs here -- it currently says "Terminals are considered to have a larger atom than any internal node" but I think that's backwards, it should say "Terminals are considered to be larger than any internal node." Because being a larger node implies "smaller atom" under the current "reversed" semantics.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:444 on 2025-08-27 15:59_

nit: What do you think about this instead?

```suggestion
            // See https://github.com/astral-sh/ruff/pull/20098 for an explanation of why this
            // ordering is reversed.
            self.interiors[a].atom.cmp(&self.interiors[b].atom).reverse()
```

To me that stands out more clearly that we are doing a reversal, rather than having to notice that `a` and `b` are reversed compared to how they appear in the parameter list.

(I'm also completely fine leaving this as-is if you prefer it that way)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:429 on 2025-08-27 16:11_

How about something like

```
Implements the ordering that determines which level a TDD node appears at.

Each interior node checks the value of a single variable (for us, a `Predicate`).
TDDs are ordered such that every path from the root of the graph to the leaves
must check each variable at most once, and must check each variable in the same
order.

We can choose any ordering that we want, as long as it's consistent — with the
caveat that terminal nodes must always be last in the ordering, since they are
the leaf nodes of the graph.

We currently compare interior nodes by looking at the Salsa IDs of each variable's
`Predicate`, since this is already available and easy to compare. We also
_reverse_ the comparison of those Salsa IDs. The Salsa IDs are assigned roughly
sequentially while traversing the source code. Reversing the comparison means
`Predicate`s that appear later in the source will tend to be placed "higher"
(closer to the root) in the TDD graph. We have [found empirically](https://github.com/astral-sh/ruff/pull/20098)
that this leads to smaller TDD graphs, since there are often repeated
combinations of `Predicate`s from earlier in the file.
```

(You'd have to reflow that to appease the pre-commit gremlin)

---

_@dcreager reviewed on 2025-08-27 16:13_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-27 16:14_

---

_@sharkdp reviewed on 2025-08-27 18:13_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:429 on 2025-08-27 18:13_

> How about something like

That is great, thank you!



---

_@sharkdp reviewed on 2025-08-27 18:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:444 on 2025-08-27 18:16_

I changed the `.reverse()` call to `b.cmp(a)` because I was worried about it being an additional operation, since this function is called *a lot*. But it's probably negligible compared to the hashmap operations and everything else that we do in `add_{and,or}_constraint`. And maybe it's a no-op after optimization anyway.

Changed back to `.reverse()` now.

---

_Merged by @sharkdp on 2025-08-27 18:42_

---

_Closed by @sharkdp on 2025-08-27 18:42_

---

_Branch deleted on 2025-08-27 18:42_

---

_Label `great writeup` added by @zanieb on 2025-08-28 00:45_

---
