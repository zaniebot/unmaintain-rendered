```yaml
number: 14621
title: "[red-knot] Fix merged type after if-else without explicit else branch"
type: pull_request
state: merged
author: sransara
labels:
  - ty
assignees: []
merged: true
base: main
head: gh-14593-inverse-narrowing-without-explicit-else-branches
created_at: 2024-11-27T03:54:05Z
updated_at: 2024-11-28T20:11:31Z
url: https://github.com/astral-sh/ruff/pull/14621
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Fix merged type after if-else without explicit else branch

---

_Pull request opened by @sransara on 2024-11-27 03:54_

## Summary

Closes: https://github.com/astral-sh/ruff/issues/14593

The final type of a variable after if-statement without explicit else branch should  be similar to having an explicit else branch.

## Test Plan

Originally failed test cases from the bug are added.


---

_Review requested from @carljm by @sransara on 2024-11-27 03:54_

---

_Review requested from @MichaReiser by @sransara on 2024-11-27 03:54_

---

_Review requested from @AlexWaygood by @sransara on 2024-11-27 03:54_

---

_Review requested from @sharkdp by @sransara on 2024-11-27 03:54_

---

_Comment by @github-actions[bot] on 2024-11-27 04:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sransara reviewed on 2024-11-27 04:53_

---

_Review comment by @sransara on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:795 on 2024-11-27 04:53_

Duplicated the logic in above `for` loop of `node.elif_else_clauses` in here. Another approach would be to have a dummy `else` clause. I wasn't sure if a dummy node with a made up range would cause issues down the line. But I can switch if that pattern is preferred than duplicating.

---

_Label `red-knot` added by @MichaReiser on 2024-11-27 06:19_

---

_@penguinolog reviewed on 2024-11-27 08:10_

---

_Review comment by @penguinolog on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/elif_else.md`:59 on 2024-11-27 08:10_

IMHO, need also test for `x` is not changed in the `if` branch like:
```python
def optional_int() -> int | None: ...

x = optional_int()

if x is None: pass

reveal_type(x)  # revealed: int | None
```

---

_@sransara reviewed on 2024-11-27 16:05_

---

_Review comment by @sransara on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/elif_else.md`:59 on 2024-11-27 16:05_

Good idea. I didn't see it being tested else where. I added another variable in the same mdtest here. I wonder if a new mdtest file would be better to test `post if block` narrowing scenarios.

---

_@sharkdp reviewed on 2024-11-27 16:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/elif_else.md`:59 on 2024-11-27 16:34_

> I wonder if a new mdtest file would be better to test `post if block` narrowing scenarios.

I think that's a good idea. We should also add tests for scenarios with (multiple) `elif` branches, but no `else` branch.

---

_Review comment by @sransara on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/elif_else.md`:59 on 2024-11-28 04:48_

I added a new test file and added couple of more tests.

---

_@sransara reviewed on 2024-11-28 04:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/post_if_statement.md`:24 on 2024-11-28 05:42_

It looks like `y` is not used in this test?
```suggestion
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/post_if_statement.md`:18 on 2024-11-28 05:42_

```suggestion
## Narrowing can have a persistent effect if the variable is mutated in one branch
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/post_if_statement.md`:3 on 2024-11-28 05:43_

```suggestion
## After if-else statements, narrowing has no effect if the variable is not mutated in any branch
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/post_if_statement.md`:34 on 2024-11-28 05:43_

```suggestion
## An if statement without an explicit `else` branch is equivalent to one with a no-op `else` branch
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/post_if_statement.md`:52 on 2024-11-28 05:44_

```suggestion
## An if-elif without an explicit else branch is equivalent to one with an empty else branch
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:795 on 2024-11-28 05:52_

Constructing an actual dummy node will cause issues, and I don't think it's necessary. But I do think we _could_ remove the logic duplication here. Instead of iterating over clauses above, we could iterate over `(test, body)` pairs, where both elements of the pair are `Option`. For a normal `else` clause, `test` would be `None` but `body` would be `Some`; if there's no `else` clause, we'd insert a `(None, None)` at the end.

That said, without trying to write it, I'm not sure if the code necessary to do this might end up being larger / harder to understand than the duplication you currently have here. Open to whatever you think is best, having worked on this.

---

_@carljm reviewed on 2024-11-28 05:53_

---

_Comment by @carljm on 2024-11-28 06:05_

Thank you, this is great!

---

_Comment by @sransara on 2024-11-28 06:37_

> Constructing an actual dummy node will cause issues, and I don't think it's necessary. But I do think we could remove the logic duplication here. Instead of iterating over clauses above, we could iterate over (test, body) pairs, where both elements of the pair are Option. For a normal else clause, test would be None but body would be Some; if there's no else clause, we'd insert a (None, None) at the end.

That's a neat idea. I think it will work. Let me try that.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:787 on 2024-11-28 12:02_

nit

```suggestion
                for (clause_test, clause_body) in elif_else_clauses {
```

---

_@AlexWaygood reviewed on 2024-11-28 12:04_

---

_@sransara reviewed on 2024-11-28 12:06_

---

_Review comment by @sransara on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:795 on 2024-11-28 12:06_

Thank you for the suggestion. This is better. Slight change I did was with `body`. It is a vector/slice. So I opted to using empty slice instead if `Option`. I have updated the code now.

---

_@sransara reviewed on 2024-11-28 12:08_

---

_Review comment by @sransara on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:787 on 2024-11-28 12:08_

Thank you!

---

_Comment by @codspeed-hq[bot] on 2024-11-28 12:13_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/sransara:gh-14593-inverse-narrowing-without-explicit-else-branches)

### Merging #14621 will **not alter performance**

<sub>Comparing <code>sransara:gh-14593-inverse-narrowing-without-explicit-else-branches</code> (864acb6) with <code>main</code> (6f1cf5b)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_@AlexWaygood reviewed on 2024-11-28 12:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:785 on 2024-11-28 12:18_

couple more code-style nits

```suggestion
                    .map(|clause| (clause.test.as_ref(), clause.body.as_slice()));
                let has_else = node
                    .elif_else_clauses
                    .last()
                    .is_some_and(|clause| clause.test.is_none());
                let elif_else_clauses = elif_else_clauses.chain(if has_else {
                    // if there's an `else` clause already, we don't need to add another
                    None
                } else {
                    // if there's no `else` branch, we should add a no-op `else` branch
                    Some((None, [].as_slice()))
```

---

_@MichaReiser reviewed on 2024-11-28 12:25_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:787 on 2024-11-28 12:25_

```suggestion
                let elif_else_clauses = node
                    .elif_else_clauses
                    .iter()
                    .map(|clause| (clause.test.as_ref(), &clause.body[..]));
                let has_else = node
                    .elif_else_clauses
                    .last()
                    .is_some_and(|clause| clause.test.is_none());
                let elif_else_clauses = elif_else_clauses.chain(if has_else {
                    // if there's an `else` clause already, we don't need to add another
                    None
                } else {
                    // if there's no `else` branch, we should add a no-op `else` branch
                    Some((None, Default::default()))
                });
                for (clause_test, clause_body) in elif_else_clauses {
```

---

_@MichaReiser reviewed on 2024-11-28 12:30_

There seems to be a somewhat significant performance regression. Could we avoid taking a post_clauses snapshot when e.g. the constraints are empty?

---

_Review comment by @sransara on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:787 on 2024-11-28 12:32_

Thank you. Previous Updates from Github UI conflicted with this, preventing updating from the UI. So I made the change and committed locally.

---

_@sransara reviewed on 2024-11-28 12:32_

---

_Comment by @sransara on 2024-11-28 12:34_

> There seems to be a somewhat significant performance regression. Could we avoid taking a post_clauses snapshot when e.g. the constraints are empty?

I'm taking a look. Actually my previous commit with collecting to vector [29014a8](https://github.com/astral-sh/ruff/pull/14621/commits/29014a874a6342db1cb5043c90ac9cab9693d8fb) didn't complain about perf regression. I wonder being fancy with iter chains is actually slowing it down.

---

_Comment by @AlexWaygood on 2024-11-28 12:36_

> Actually my previous commit with collecting to vector [29014a8](https://github.com/astral-sh/ruff/pull/14621/commits/29014a874a6342db1cb5043c90ac9cab9693d8fb) didn't complain about perf regression. I wonder being fancy with iter chains is actually slowing it down.

yeah, this is pretty surprising. I would expect collecting into a vector to be expensive, and using an iter chain to be relatively cheap. Not sure what's going on here.

---

_Comment by @AlexWaygood on 2024-11-28 12:39_

I think what might be going on is that there was always a performance regression, but it was right on the borderline of the arbitrary 4% line that codspeed uses to determine whether or not to comment on the PR. So purely by chance, previous measurements on the PR "got lucky" due to random noise in the benchmarks, and codspeed never commented on the PR. But the penultimate measurement that codspeed made went _just_ over the line, and cause it to comment and fail the check.

The check is now passing again, but you can see that there is still a perf regression in the benchmaks that's reasonably significant, it's just once again not _quite_ big enough for codspeed to complain about it: https://codspeed.io/astral-sh/ruff/branches/sransara:gh-14593-inverse-narrowing-without-explicit-else-branches

---

_Comment by @sransara on 2024-11-28 12:44_

That makes sense. Looking into @MichaReiser's suggestion.

---

_@carljm approved on 2024-11-28 14:17_

Very nice!!

---

_Comment by @carljm on 2024-11-28 14:22_

I don't think Micha's suggestion to bring down the regression is possible. At this stage (semantic indexing) there is always at least one constraint; it's simply the test expression(s) of the `if` and each `elif`, stored for later analysis. We can't know until type inference whether those test expressions apply any actual narrowing constraints. 

I think this falls into the general bucket we are already aware of that snapshots are expensive and we may need to improve perf of constructing the use def map. Short of tackling that larger project, I think we just have to accept this regression as the cost of correct semantics here. 

So I will go ahead and merge this as is. Thank you for the PR!

---

_Merged by @carljm on 2024-11-28 14:23_

---

_Closed by @carljm on 2024-11-28 14:23_

---

_Comment by @sransara on 2024-11-28 20:11_

Thank you for the review and merge!

Looking at the restore and snapshot implementation, I can see how the `symbol_states` can keep growing to a point that `IndexVec.clone` become an expensive operation.

---
