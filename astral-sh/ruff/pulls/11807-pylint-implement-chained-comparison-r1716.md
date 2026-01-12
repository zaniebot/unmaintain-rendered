```yaml
number: 11807
title: "[`pylint`] Implement `chained-comparison` (`R1716`)"
type: pull_request
state: closed
author: max-muoto
labels:
  - rule
assignees: []
base: main
head: unnecessary-chained-comprehension
created_at: 2024-06-09T00:56:17Z
updated_at: 2024-09-07T23:10:42Z
url: https://github.com/astral-sh/ruff/pull/11807
synced_at: 2026-01-12T15:55:39Z
```

# [`pylint`] Implement `chained-comparison` (`R1716`)

---

_@max-muoto_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements the `chained-comparison` rule from [Pylint](https://pylint.readthedocs.io/en/stable/user_guide/messages/refactor/chained-comparison.html).

Currently this doesn't include an autofix, but this is something I'll try and follow-up with.

## Test Plan

<!-- How was it tested? -->

`cargo test`.


---

_Renamed from "chore: Scaffolding" to "Implement Unnecessary Chained Comprehension" by @max-muoto on 2024-06-09 00:56_

---

_Renamed from "Implement Unnecessary Chained Comprehension" to "Implement Unnecessary Chained Comparison" by @max-muoto on 2024-06-09 03:54_

---

_Comment by @github-actions[bot] on 2024-06-09 04:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 48 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/colors/util.py#L63'>src/bokeh/colors/util.py:63:16:</a> PLR1716 Simplified chain comparison exists between the operands.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/0ca0e036725ccda9107c4931aba353552d0f10cf/zilencer/management/commands/queue_rate.py#L59'>zilencer/management/commands/queue_rate.py:59:24:</a> PLR1716 Simplified chain comparison exists between the operands.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1716 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Renamed from "Implement Unnecessary Chained Comparison" to "[`pylint`] Implement `chained-comparison` (`R1716`)" by @max-muoto on 2024-06-09 04:40_

---

_Marked ready for review by @max-muoto on 2024-06-09 04:42_

---

_Converted to draft by @max-muoto on 2024-06-09 05:11_

---

_Marked ready for review by @max-muoto on 2024-06-09 05:26_

---

_Label `rule` added by @MichaReiser on 2024-06-10 06:54_

---

_Comment by @max-muoto on 2024-06-17 15:47_

Just wanted to follow-up on this, maybe @charliermarsh if you're able to review?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:98 on 2024-06-20 07:23_

Cloning all IDs is rather expensive. We can avoid it by introducing some lifetimes. 

```patch
Subject: [PATCH] Don't lazily infer imports because it adds an AST dependency to the module looking up the definition
---
Index: crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs b/crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs
--- a/crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs	(revision dbfcdefddeea5496c84530684429019fba4bff87)
+++ b/crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs	(date 1718868097754)
@@ -47,12 +47,12 @@
     upper_bound: HashSet<i32>,
 }
 
-fn update_bounds(
+fn update_bounds<'a>(
     operator: ast::CmpOp,
-    id: String,
+    id: &'a str,
     node_id: i32,
     is_left: bool,
-    uses: &mut HashMap<String, Bounds>,
+    uses: &mut HashMap<&'a str, Bounds>,
 ) {
     match operator {
         ast::CmpOp::Lt | ast::CmpOp::LtE if is_left => {
@@ -71,16 +71,16 @@
     }
 }
 
-fn set_lower_upper_bounds(node: &ast::ExprCompare, uses: &mut HashMap<String, Bounds>) {
+fn set_lower_upper_bounds<'a>(node: &'a ast::ExprCompare, uses: &mut HashMap<&'a str, Bounds>) {
     let mut left_operand: &ast::Expr = &node.left;
     let node_id = node as *const _ as i32;
     for (right_operand, operator) in node.comparators.iter().zip(node.ops.iter()) {
         if let Some(left_name_expr) = left_operand.as_name_expr() {
-            update_bounds(*operator, left_name_expr.id.clone(), node_id, true, uses);
+            update_bounds(*operator, &left_name_expr.id, node_id, true, uses);
         }
 
         if let Some(right_name_expr) = right_operand.as_name_expr() {
-            update_bounds(*operator, right_name_expr.id.clone(), node_id, false, uses);
+            update_bounds(*operator, &right_name_expr.id, node_id, false, uses);
         }
 
         left_operand = right_operand;
@@ -95,7 +95,7 @@
         return;
     }
 
-    let mut uses: HashMap<String, Bounds> = HashMap::new();
+    let mut uses: HashMap<&str, Bounds> = HashMap::new();
     for expr in values {
         let Some(compare_expr) = expr.as_compare_expr() else {
             return;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:87 on 2024-06-20 07:30_

I don't think this is correct and I'm surprised that Clippy doesn't complain about a possible truncation. 

* `node` is a pointer. The size of a pointer depends on the platform but is 8 bytes large on 64bit systems
* `i32` is 4 bytes large

Maybe @BurntSushi can enlighten me.

Anyway. I think we can get away with something simpler. We don't need globally unique node ids. Instead, we can just use an increasing index:

```patch
Subject: [PATCH] Don't lazily infer imports because it adds an AST dependency to the module looking up the definition
---
Index: crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs b/crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs
--- a/crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs	(revision dbfcdefddeea5496c84530684429019fba4bff87)
+++ b/crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs	(date 1718868630142)
@@ -43,14 +43,14 @@
 // Each integer is a unique identifier for the node.
 #[derive(Default)]
 struct Bounds {
-    lower_bound: HashSet<i32>,
-    upper_bound: HashSet<i32>,
+    lower_bound: HashSet<u32>,
+    upper_bound: HashSet<u32>,
 }
 
 fn update_bounds(
     operator: ast::CmpOp,
     id: String,
-    node_id: i32,
+    node_id: u32,
     is_left: bool,
     uses: &mut HashMap<String, Bounds>,
 ) {
@@ -71,9 +71,13 @@
     }
 }
 
-fn set_lower_upper_bounds(node: &ast::ExprCompare, uses: &mut HashMap<String, Bounds>) {
+fn set_lower_upper_bounds(
+    node: &ast::ExprCompare,
+    node_id: u32,
+    uses: &mut HashMap<String, Bounds>,
+) {
     let mut left_operand: &ast::Expr = &node.left;
-    let node_id = node as *const _ as i32;
+
     for (right_operand, operator) in node.comparators.iter().zip(node.ops.iter()) {
         if let Some(left_name_expr) = left_operand.as_name_expr() {
             update_bounds(*operator, left_name_expr.id.clone(), node_id, true, uses);
@@ -96,11 +100,13 @@
     }
 
     let mut uses: HashMap<String, Bounds> = HashMap::new();
+    let mut node_id = 0u32;
     for expr in values {
         let Some(compare_expr) = expr.as_compare_expr() else {
             return;
         };
-        set_lower_upper_bounds(compare_expr, &mut uses);
+        set_lower_upper_bounds(compare_expr, node_id, &mut uses);
+        node_id += 1;
     }
 
     for bound in uses.values() {
```


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:48 on 2024-06-20 07:39_

I'm having some trouble understanding the precise logic, but I wonder if it is worth paying the cost of allocating two hash sets for each `Bound`. Especially, what's unclear to me is if the uniqueness is important for the correctness of the rule. Ultimately, we only iterate over the nodes once. That means we should never end up inserting the same node twice into lower or upper bounds. 

I think we could instead use

* Use a `Vec<(u32, Bound)>` where `enum Bound { Lower, Upper }`
* We also know that the vec is sorted because we iterate the nodes in order. We can then implement a manual counting and use lookahead to to test if an id does both an upper and lower bound check

This has the downside that we might end up writing a few extra bounds if a comparison tests the same name multiple times but that's fine. The overhead for this is minimal. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:124 on 2024-06-20 07:42_

I think it would be good if the diagnostic at least explains which part can be simplified and how. The violation otherwise seems hard to understand because Ruff tells you that you could do better without telling you how.

---

_@MichaReiser reviewed on 2024-06-20 07:44_

Thanks for contributing this rule. 

I left  few comments on how I think we could improve the rule's performance. 

I think we may want to spend some more time on improving the message. I as a user would struggle to understand what I'm supposed to do. Would it be possible to at least mention the relevant variables? 

---

_@BurntSushi reviewed on 2024-06-20 11:31_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:87 on 2024-06-20 11:31_

Yeah, it definitely could be a truncation here. I would in general be very careful about casting pointers to integers. I like your diff @MichaReiser. :)

---

_Comment by @max-muoto on 2024-06-20 15:32_

> Thanks for contributing this rule.
> 
> I left few comments on how I think we could improve the rule's performance.
> 
> I think we may want to spend some more time on improving the message. I as a user would struggle to understand what I'm supposed to do. Would it be possible to at least mention the relevant variables?

Thanks for the comprehensive review! I'll try and get back to everything tonight.

---

_@alexcdennis reviewed on 2024-06-21 16:12_

---

_Review comment by @alexcdennis on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:101 on 2024-06-21 16:12_

`continue` instead of `return`
to support test case like `a < b and True and b < c`

---

_@alexcdennis reviewed on 2024-06-21 16:30_

---

_Review comment by @alexcdennis on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:48 on 2024-06-21 16:30_

could avoid the allocation altogether for `bool_op.values.len() <= 32` by using bit sets for `lower_bound` and `upper_bound`

---

_@max-muoto reviewed on 2024-06-23 02:38_

---

_Review comment by @max-muoto on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:101 on 2024-06-23 02:38_

Fixed: https://github.com/astral-sh/ruff/pull/11807/commits/6ee4118a9fc3419069b729bff50fd9bb18059f46

---

_@max-muoto reviewed on 2024-06-23 02:39_

---

_Review comment by @max-muoto on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:87 on 2024-06-23 02:39_

Great catch, no reason not to use an index here: https://github.com/astral-sh/ruff/pull/11807/commits/6ee4118a9fc3419069b729bff50fd9bb18059f46

---

_@max-muoto reviewed on 2024-06-23 02:39_

---

_Review comment by @max-muoto on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:98 on 2024-06-23 02:39_

Thanks, good callout, fixed!

---

_Comment by @codspeed-hq[bot] on 2024-06-23 02:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/max-muoto:unnecessary-chained-comprehension)

### Merging #11807 will **degrade performances by 4.83%**

<sub>Comparing <code>max-muoto:unnecessary-chained-comprehension</code> (f60f481) with <code>main</code> (7156096)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/max-muoto:unnecessary-chained-comprehension)._

### Benchmarks breakdown

|     | Benchmark | `main` | `max-muoto:unnecessary-chained-comprehension` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 1.8 ms | 1.9 ms | -4.83% |


---

_Review comment by @max-muoto on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:48 on 2024-06-23 03:12_

Great idea around using a bit-set, let me know what you think: https://github.com/astral-sh/ruff/pull/11807/commits/eb08ee497eeeaa4d0ef78f80ba4192be0617a4ad

---

_@max-muoto reviewed on 2024-06-23 03:12_

---

_@max-muoto reviewed on 2024-06-23 03:30_

---

_Review comment by @max-muoto on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:124 on 2024-06-23 03:30_

I can add that in now, or can wait until I add the autofix as well, which might make more sense as then I can consolidate how the new representation is determined. Any thoughts?

---

_Review comment by @alexcdennis on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:75 on 2024-06-24 20:37_

now that you are using a bit-set, these will fail for `node_idx >= 32`

I recommend having a check at the top of `unneccessary_chained_comparison` for `bool_op.values.len() > 32`

I'm guessing we want to keep the HashMap implementation as a fallback for longer expressions

---

_@alexcdennis reviewed on 2024-06-24 20:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:73 on 2024-06-28 07:36_

Using bit-index here is clever, but I think it's a bit too clever and we can probably get a way with a simpler solution that also works when there are more than 32 nodes.

Correct me if I'm wrong, but I think the analysis has a few interesting properties

* The `node_idx` is always increasing (it's sorted!)
* The only data that we need to compute is the `shared_bounds`, `lower_bounds_count`, and `upper_bounds_count`

I wonder if we could use these properties to do the calculation right here in `update_bounds`:

What if we stored the following data for each identifier:

```rust
struct Bounds {
	lower_count: u32,
	upper_count: u32,
	shared_count: NonZeroU32,
	last_node_index: u32
}
```

* a lower bound increments `lower_count`
* an upper bound increments `upper_count`
* shared count gets updated when `last_node_index == node_idx`
* `update_bounds` always sets `last_node_index`. 



---

_@MichaReiser reviewed on 2024-06-28 07:36_

---

_@MichaReiser reviewed on 2024-06-28 07:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:124 on 2024-06-28 07:37_

@AlexWaygood what's your take. Do you have an idea on how to improve the message without including the identifier name?

---

_@alexcdennis reviewed on 2024-06-28 14:24_

---

_Review comment by @alexcdennis on `crates/ruff_linter/src/rules/pylint/rules/unnecessary_chained_comparison.rs`:73 on 2024-06-28 14:24_

We just have to be careful with non-monotonic/non-totally-ordered comparisons (eg. `a < b > c and b < d`). The original pylint rule triggers on this case (although it's not clear what the correct fix is or if it is even appropriate to trigger on this case (relevant: #10028)).

---

_Comment by @max-muoto on 2024-09-07 23:10_

Closing until I have more time to look into this, and properly address feedback.

---

_Closed by @max-muoto on 2024-09-07 23:10_

---
