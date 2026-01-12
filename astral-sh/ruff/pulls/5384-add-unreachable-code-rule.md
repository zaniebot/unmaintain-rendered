```yaml
number: 5384
title: Add unreachable code rule
type: pull_request
state: merged
author: Thomasdezeeuw
labels: []
assignees: []
merged: true
base: main
head: thomas/control_flow_graph
created_at: 2023-06-27T10:21:45Z
updated_at: 2023-07-04T14:42:47Z
url: https://github.com/astral-sh/ruff/pull/5384
synced_at: 2026-01-12T15:55:18Z
```

# Add unreachable code rule

---

_@Thomasdezeeuw_

## Summary

This adds a new rule that detect unreachable code, currently limited to function bodies.

How it Works
============

The rule works as follows.

First we create "basic blocks" from the statements. These basic block are zero or more lines of code (statements) for which the code flow is easy to follow. Specifically all statements in a single block follow each other, no diversion of the control flow. At the end of the block the control can do one of three things: 1) continue to another code block, 2) based on a condition jump to one of two code blocks, or 3) terminate (return or end of the function).

Second, based on these basic blocks, and the simplified control flow they represent, we can determine what blocks are and aren't reached. We do this by starting with the first block of the function and following the jumps it makes to the next code block, marking them all as reached.

Third, we create a diagnostic for each code block that is not reached by step 2. Currently these diagnostics are quite limited, see below.

Future Work and Limitations
===========================

This commit is only the beginning of this rule, there is much work still left to do.

The diagnostics created currently is quite limited. It only mentions the function name and points to the fist statement in the basic block. In the future this should be expanded to point to all statements in the basic block. Furthermore it would be helpful to the users to explain *why* a code block is not reached as currently, except for the most basic example, this might not be clear.

We're currently quite limited on how we determine if a branch is always taken or not, specifically we only detect the constants true and false and only in the most basic condition (mainly the `if` and `while` statements). This can be expanded to detect more cases where we can statically determine whether or not a branch is taken.

This currently doesn't handle the `try` or (`async`) `with` statements.

For match statements we currently set `BasicBlock::stmts` to the entire match statement for each basic block, even though we're only interested in one of its cases.

Within match statements binding to named patterns is currently not handled. Similarly to wildcard they should be considered to be always taken (assuming no guard is present).

False Positive
===========

This has the possibility for false positive mostly around the non-implementation of `try` and `with` statements. Currently I found one in the Bokeh repo and added it as a (commented-out) test case. I believe time is best spend implementing `try` and `with` instead of working around this issue.

## Test Plan

Added new tests, simply run `cargo test`.

I also manually ran this on airflow the Bokeh, Cpython, FastAPI and Jupyter server repos.

---

_Comment by @github-actions[bot] on 2023-06-27 10:53_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.1±0.05ms     5.0 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.01   1768.6±5.14µs     9.4 MB/sec    1.00   1745.5±2.57µs     9.5 MB/sec
formatter/numpy/globals.py                 1.02    198.3±0.43µs    14.9 MB/sec    1.00    195.1±0.34µs    15.1 MB/sec
formatter/pydantic/types.py                1.02      3.9±0.02ms     6.6 MB/sec    1.00      3.8±0.01ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.02     14.1±0.08ms     2.9 MB/sec    1.00     13.8±0.06ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.5±0.02ms     4.7 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    436.4±1.49µs     6.8 MB/sec    1.00    435.6±0.39µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.3±0.02ms     4.1 MB/sec    1.00      6.1±0.03ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.0±0.02ms     5.8 MB/sec    1.00      6.9±0.02ms     5.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1495.1±2.75µs    11.1 MB/sec    1.00   1499.9±3.07µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.8±3.71µs    17.3 MB/sec    1.00    170.7±0.26µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.02ms     8.2 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.6±0.12ms     4.2 MB/sec    1.00      9.4±0.07ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.0±0.02ms     8.3 MB/sec    1.00      2.0±0.02ms     8.3 MB/sec
formatter/numpy/globals.py                 1.00    218.9±2.29µs    13.5 MB/sec    1.02   222.4±14.54µs    13.3 MB/sec
formatter/pydantic/types.py                1.01      4.5±0.04ms     5.7 MB/sec    1.00      4.5±0.04ms     5.7 MB/sec
linter/all-rules/large/dataset.py          1.00     15.4±0.16ms     2.6 MB/sec    1.00     15.5±0.07ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.0 MB/sec    1.00      4.1±0.03ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00   429.0±10.25µs     6.9 MB/sec    1.01    433.8±7.69µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.12ms     3.6 MB/sec    1.01      7.1±0.16ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.03ms     5.0 MB/sec    1.02      8.2±0.04ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1658.9±9.63µs    10.0 MB/sec    1.01   1676.2±8.92µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.5±1.34µs    16.4 MB/sec    1.00    181.1±1.32µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.0 MB/sec    1.01      3.7±0.02ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @Thomasdezeeuw on 2023-06-27 11:51_

I found a couple of false positive with the Bokeh repo, looking into them now.

---

_Review requested from @MichaReiser by @Thomasdezeeuw on 2023-06-27 13:04_

---

_Review requested from @charliermarsh by @Thomasdezeeuw on 2023-06-27 13:04_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:207 on 2023-06-28 12:37_

Not necessarily for this PR but I think it would make sense to extract `BasicBlock`s because they are useful for all sort of control flow computation. For example, we could use it to detect uninitialized/potentially uninitialized variable reads.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:197 on 2023-06-28 12:39_

I don't understand this part. Which statement is *the statement*? And how's the *last statement* in a function defined?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:224 on 2023-06-28 12:42_

What's the reason for creating a connection to `block 0` vs creating four blocks: 

1. Everything before the while `i = 0`
2. While header (`while True:`)
3. While body
4. After the body

And connecting

* 1 and 2 with an unconditional jump
* 2 and 3 with a conditional jump
* 2 and 4 with a conditional jump
* 3 and 2 with an unconditional jump

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:223 on 2023-06-28 12:45_

Nit: We can use [`IndexVec`](https://github.com/astral-sh/ruff/blob/dd8fc4876ae590e0d906e05717b7a68c3e1a3244/crates/ruff_index/src/vec.rs#L11) here 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:335 on 2023-06-28 12:46_

Is there a risk that creating a sentinel return statement drips over any analysis or diagnostic rendering?

Edit: Okay. I guess you already handle this by `is_sentinel`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:362 on 2023-06-28 12:48_

Nit: Does this comment apply for `Break` and `Continue`. If so, I think it would make sense to move these two instruction into their own match arm even when they both have the same body.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:174 on 2023-06-28 12:54_

What are your thoughts on using a Vec containing the blocks that need visiting vs using recursion?

---

_@MichaReiser reviewed on 2023-06-28 12:57_

Impressive work! I've two questions that may as well fall under *future work*

* Have you thought about how to support e.g. conditional expressions where we also have a conditional data flow?

* I commented on the `BasicBlock` layout. Could you expand on the reason why you chose this specific `BasicBlock` layout? Or in general, could you document how the basic blocks should be structured?

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:174 on 2023-06-29 09:20_

That would work as well. I mainly chose recursion for simplicity and I didn't want to allocate for a single index, which might be common for small functions. If we go with the `vec` route we could think about something like `smallvec` (https://crates.io/crates/smallvec) to avoid small heap allocations.

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:362 on 2023-06-29 09:21_

No only for `Continue`, break should stop looping and continue to the statement after the loop. `Break` however should return to the loop statement (creating the loop).

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:335 on 2023-06-29 09:22_

Sentinel blocks are printed in the graph, which looks a little odd at times, but indeed in the diagnostics they shouldn't appear.

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:224 on 2023-06-29 09:33_

For the purposes of detecting unreachable code, your block 1 doesn't contain any control flow, notwithstanding function calls that always raise an exception, thus block 2 is always reached when block 1 is reached. Creating one block instead of two is then simply less work for us later on. Rustc does the same thing, see https://rustc-dev-guide.rust-lang.org/appendix/background.html#what-is-a-control-flow-graph.

Other then the addition block, it's what we roughly create at the moment, see https://github.com/astral-sh/ruff/pull/5384/files#diff-a98f67bee97e7c459fad838467a76c7cbcd7c66beca1bae4826d1fa4054c4e5d (crates/ruff/src/rules/ruff/rules/snapshots/ruff__rules__ruff__rules__unreachable__tests__while.py_12.snap) for a graph of code that is quite similar.

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:197 on 2023-06-29 09:39_

> I don't understand this part. Which statement is _the statement_?

While changing the docs I forgot a crucial word:

```suggestion
    /// the last block is the first statement in the function and the first block is
```

> And how's the _last statement_ in a function defined?

It's whatever the last statements in in the list of `Stmt` in the call to `BasicBlocks::from(&stmts)`. In other words the last line of code in the function (at the 1st level of indentation, i.e. the function body level of indentation). For example

```python
def function():
    while True: # Last line.
        continue # (part of the `while` statement)
```

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:207 on 2023-06-29 09:39_

Indeed, I didn't quite know where to put them, so I though I just put them in the rule file for now, makes avoiding merge conflicts a whole lot easier as well.

---

_@Thomasdezeeuw reviewed on 2023-06-29 09:56_

> Impressive work! I've two questions that may as well fall under _future work_
> 
>     * Have you thought about how to support e.g. conditional expressions where we also have a conditional data flow?

You mean for example an `if` statement inside of the condition (test) of an `if` statement (`Expr::IfExp`)? No, I didn't have time to look at this yet.

>     * I commented on the `BasicBlock` layout. Could you expand on the reason why you chose this specific `BasicBlock` layout? Or in general, could you document how the basic blocks should be structured?

I'm going to assume you mean `BasicBlocks` (multiple) as you left a comment on that.

Basically the `BasicBlocks.blocks` is a tree laid out as a vector/array. The fist and last blocks are defined as the last and first blocks in the function (i.e. they are switched) because we start processing the last statement. Why start with the last statement? Because the statement always jumps to the next statement assuming no control flow diversion, which means they have to be part of the existing tree (vector) to reference them.

In between these two block however things get a little fuzzy... It's not ideal, but I couldn't really find a reasonable way to make it return a fixed order (in the time I had). Basically each statement can add an arbitrary number of blocks as it can have an arbitrary number of statements within it (think the body of a loop or if statement). For these "sub-statements" we use the same approach as the top-level statements, but we reuse the `blocks` vector (for performance reasons). This all works fairly straight forward up to the point where you have recursion, e.g. for loops.

For while loops the body jumps to the while block itself, unless we see a `break` or `return`. But by default `create_blocks` points to the last block in `blocks` as the next block (unless `after` is set). But the `after` argument isn't sufficient in all cases, so we need `change_next_block` to fix up the control flow in some cases. (I added the `after` argument after I already implemented `change_next_block`, but I couldn't remove it as it's still required in some cases last time I checked)

---

_@dimaqq reviewed on 2023-06-29 13:03_

Consider special-casing `yield`

```py
>>> def foo():
...   if False: yield
...   return 42
... 
>>> def bar():
...   return 42
... 
...
>>> foo()
<generator object foo at 0x7ffa840a4e00>
>>> bar()
42
```

IIRC there's one more corner case with nonlocals/locals in the presence of a closure, but I can't type if off the top of my head.

---

_Comment by @Thomasdezeeuw on 2023-07-02 15:32_

> Consider special-casing `yield`
> 
> ```python
> >>> def foo():
> ...   if False: yield
> ...   return 42
> ... 
> >>> def bar():
> ...   return 42
> ... 
> ...
> >>> foo()
> <generator object foo at 0x7ffa840a4e00>
> >>> bar()
> 42
> ```
> 
> IIRC there's one more corner case with nonlocals/locals in the presence of a closure, but I can't type if off the top of my head.

I've moved `Stmt::Expr` to be handled separately, but it's future work.

---

_@Thomasdezeeuw reviewed on 2023-07-02 15:33_

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:223 on 2023-07-02 15:33_

I tried doing this, but `ruff` doesn't seem to dependent on `ruff_index` currently. Is is worth added the dependency just for this one type?

---

_Comment by @Thomasdezeeuw on 2023-07-02 15:38_

@MichaReiser @charliermarsh I can't push the branch any more, but here are two more patches:

Move Stmt::Expr to a different match branch:

````patch
From d2042fbd090cd59e77363395a48dc94dd6da0db0 Mon Sep 17 00:00:00 2001
From: Thomas de Zeeuw <thomas@astral.sh>
Date: Sun, 2 Jul 2023 17:28:09 +0200
Subject: [PATCH 1/2] Move Stmt::Expr to a different match branch

This still needs to be handled.
---
 .../ruff/src/rules/ruff/rules/unreachable.rs  | 34 ++++++++++++++++++-
 1 file changed, 33 insertions(+), 1 deletion(-)

diff --git a/crates/ruff/src/rules/ruff/rules/unreachable.rs b/crates/ruff/src/rules/ruff/rules/unreachable.rs
index b8959749b..5bc085cf8 100644
--- a/crates/ruff/src/rules/ruff/rules/unreachable.rs
+++ b/crates/ruff/src/rules/ruff/rules/unreachable.rs
@@ -357,7 +357,6 @@ fn create_blocks<'stmt>(
             | Stmt::Assign(_)
             | Stmt::AugAssign(_)
             | Stmt::AnnAssign(_)
-            | Stmt::Expr(_)
             | Stmt::Break(_)
             | Stmt::Continue(_) // NOTE: the next branch gets fixed up in `change_next_block`.
             | Stmt::Pass(_) => unconditional_next_block(blocks, after),
@@ -437,6 +436,7 @@ fn create_blocks<'stmt>(
                 // TODO: currently we don't include the lines before the match
                 // statement in the block, unlike what we do for other
                 // statements.
+                after = Some(blocks.len() - 1);
                 continue;
             }
             Stmt::Raise(_) => {
@@ -461,6 +461,38 @@ fn create_blocks<'stmt>(
                     orelse,
                 }
             }
+            Stmt::Expr(stmt) => {
+                match &*stmt.value {
+                    Expr::BoolOp(_) |
+                    Expr::BinOp(_) |
+                    Expr::UnaryOp(_) |
+                    Expr::Dict(_) |
+                    Expr::Set(_) |
+                    Expr::Compare(_) |
+                    Expr::Call(_) |
+                    Expr::FormattedValue(_) |
+                    Expr::JoinedStr(_) |
+                    Expr::Constant(_) |
+                    Expr::Attribute(_) |
+                    Expr::Subscript(_) |
+                    Expr::Starred(_) |
+                    Expr::Name(_) |
+                    Expr::List(_) |
+                    Expr::Tuple(_) |
+                    Expr::Slice(_)  => unconditional_next_block(blocks, after),
+                    // TODO: handle these expressions.
+                    Expr::NamedExpr(_) |
+                    Expr::Lambda(_) |
+                    Expr::IfExp(_) |
+                    Expr::ListComp(_) |
+                    Expr::SetComp(_) |
+                    Expr::DictComp(_) |
+                    Expr::GeneratorExp(_) |
+                    Expr::Await(_) |
+                    Expr::Yield(_) |
+                    Expr::YieldFrom(_) => unconditional_next_block(blocks, after),
+                }
+            },
             // The tough branches are done, here is an easy one.
             Stmt::Return(_) => NextBlock::Terminate,
         };
-- 
2.41.0
``` 

Improve BasicBlocks docs:

````patch
From 41e7813b4d83771689905020c5cd1e0779788445 Mon Sep 17 00:00:00 2001
From: Thomas de Zeeuw <thomas@astral.sh>
Date: Sun, 2 Jul 2023 17:29:42 +0200
Subject: [PATCH 2/2] Improve BasicBlocks docs

---
 crates/ruff/src/rules/ruff/rules/unreachable.rs | 7 ++++---
 1 file changed, 4 insertions(+), 3 deletions(-)

diff --git a/crates/ruff/src/rules/ruff/rules/unreachable.rs b/crates/ruff/src/rules/ruff/rules/unreachable.rs
index 5bc085cf8..dcbe33186 100644
--- a/crates/ruff/src/rules/ruff/rules/unreachable.rs
+++ b/crates/ruff/src/rules/ruff/rules/unreachable.rs
@@ -194,9 +194,10 @@ struct BasicBlocks<'stmt> {
     /// # Notes
     ///
     /// The order of these block is unspecified. However it's guaranteed that
-    /// the last block is the statement in the function and the first block is
-    /// the last statement. The block are more or less in reverse order, but it
-    /// gets fussy around control flow statements (e.g. `if` statements).
+    /// the last block is the first statement in the function and the first
+    /// block is the last statement. The block are more or less in reverse
+    /// order, but it gets fussy around control flow statements (e.g. `while`
+    /// statements).
     ///
     /// For loop blocks, and similar recurring control flows, the end of the
     /// body will point to the loop block again (to create the loop). However an
-- 
2.41.0
``` 

---

_@MichaReiser reviewed on 2023-07-04 06:43_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:197 on 2023-07-04 06:43_

Oh interesting that the blocks are in reverse order. What's the motivation for adding them in reverse order?

---

_Comment by @MichaReiser on 2023-07-04 09:25_

Current dependencies on/for this PR:
* main
  * **PR #5384** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/5384" target="_blank"><img src="https://static.graphite.dev/graphite-32x32.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This comment was auto-generated by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/5384?utm_source=stack-comment).

---

_@MichaReiser reviewed on 2023-07-04 11:50_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/snapshots/ruff__rules__ruff__rules__unreachable__tests__async-for.py.md.snap`:2 on 2023-07-04 11:50_

I changed the file extension to `.md.snap`. This gives us a nice preview if you associate the file extension with markdown in your editor

![image](https://github.com/astral-sh/ruff/assets/1203881/9502eb76-2ed9-41a6-9883-a189d933f260)


---

_@MichaReiser approved on 2023-07-04 11:51_

---

_Comment by @MichaReiser on 2023-07-04 11:51_

The only remaining question is how to get the rule for now.

---

_@MichaReiser reviewed on 2023-07-04 14:10_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/unreachable.rs`:197 on 2023-07-04 14:10_

I saw that you answered this in your reply further down.

---

_Merged by @MichaReiser on 2023-07-04 14:27_

---

_Closed by @MichaReiser on 2023-07-04 14:27_

---

_Branch deleted on 2023-07-04 14:27_

---
