```yaml
number: 5790
title: Omit tuple parentheses inside comprehensions
type: pull_request
state: merged
author: cnpryer
labels:
  - formatter
assignees: []
merged: true
base: main
head: tuple-in-comp
created_at: 2023-07-15T23:27:26Z
updated_at: 2023-07-20T06:48:01Z
url: https://github.com/astral-sh/ruff/pull/5790
synced_at: 2026-01-12T03:30:21Z
```

# Omit tuple parentheses inside comprehensions

---

_Pull request opened by @cnpryer on 2023-07-15 23:27_

## Summary

Closes #5779 

Implements `TupleParentheses::Comprehension` for omitting parentheses inside comprehensions.

## Test Plan

Review updated snapshots.

______

I feel like this kind of thing can be implemented a number of ways. One way I was thinking about was a `NeedsParentheses` implementation. I imagined something like 
```rust
impl NeedsParentheses for ExprTuple {
    fn needs_parentheses(
        &self,
        parent: AnyNodeRef,
        _context: &PyFormatContext,
    ) -> OptionalParentheses {
        if parent.is_comprehension() {
            ...
        }
    }
}
```

I could be mixing concepts though. I'm also wondering if this warrants just having `TupleParentheses::Never`.


TODO:
- [x] [Handle parenthesized `Argument`](https://github.com/astral-sh/ruff/issues/5779#issuecomment-1637614763) https://github.com/astral-sh/ruff/pull/5790/commits/3bc182b0946e10c39eb6133b4ceb08107c35c9eb
- [x] More fixtures

---

_@cnpryer reviewed on 2023-07-15 23:28_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:131 on 2023-07-15 23:28_

This threw me off a little, so I can play around with it more.

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/other/comprehension.rs`:118 on 2023-07-15 23:32_

Should I be consistent here with a .fmt()? They're effectively the same thing, right?

---

_@cnpryer reviewed on 2023-07-15 23:32_

---

_Comment by @github-actions[bot] on 2023-07-15 23:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.05ms     4.2 MB/sec    1.01      9.8±0.04ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1904.1±5.72µs     8.7 MB/sec    1.00   1910.2±3.86µs     8.7 MB/sec
formatter/numpy/globals.py                 1.00    207.6±0.45µs    14.2 MB/sec    1.01    209.0±0.82µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.1 MB/sec    1.00      4.2±0.01ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.11ms     3.0 MB/sec    1.00     13.5±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    373.9±0.76µs     7.9 MB/sec    1.00    373.0±2.76µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.06ms     4.2 MB/sec    1.00      6.1±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      7.1±0.03ms     5.7 MB/sec    1.00      7.1±0.03ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1440.4±4.38µs    11.6 MB/sec    1.00   1434.0±5.05µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    150.5±0.38µs    19.6 MB/sec    1.00    149.3±0.49µs    19.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.02ms     8.1 MB/sec    1.00      3.2±0.01ms     8.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     10.3±0.18ms     3.9 MB/sec    1.01     10.4±0.25ms     3.9 MB/sec
formatter/numpy/ctypeslib.py               1.00  1936.9±26.87µs     8.6 MB/sec    1.01  1959.1±32.26µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    208.0±1.72µs    14.2 MB/sec    1.00    207.9±2.18µs    14.2 MB/sec
formatter/pydantic/types.py                1.00      4.3±0.07ms     5.9 MB/sec    1.01      4.3±0.08ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.0±0.48ms     2.7 MB/sec    1.02     15.3±0.43ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.13ms     4.6 MB/sec    1.05      3.8±0.18ms     4.3 MB/sec
linter/all-rules/numpy/globals.py          1.00    399.3±5.48µs     7.4 MB/sec    1.00   399.4±11.91µs     7.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.12ms     3.9 MB/sec    1.00      6.5±0.26ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.06      7.9±0.16ms     5.2 MB/sec    1.00      7.4±0.12ms     5.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.04  1535.1±21.35µs    10.8 MB/sec    1.00  1471.2±15.37µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.03    166.1±2.04µs    17.8 MB/sec    1.00    161.8±1.52µs    18.2 MB/sec
linter/default-rules/pydantic/types.py     1.04      3.3±0.03ms     7.7 MB/sec    1.00      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@cnpryer reviewed on 2023-07-16 01:58_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:17 on 2023-07-16 01:58_

Originally I wanted to do `TupleParentheses::Expr(Parentheses::Never)` but I'd need more to handle
```
# Input
h3 = 1, "qweiurpoiqwurepqiurpqirpuqoiwrupqoirupqoirupqoiurpqiorupwqiourpqurpqurpqurpqurpqurpqurüqurqpuriq"

# Ruff
h3 = 1, "qweiurpoiqwurepqiurpqirpuqoiwrupqoirupqoirupqoiurpqiorupwqiourpqurpqurpqurpqurpqurpqurüqurqpuriq"

# Black
+h3 = (
+    1,
+    "qweiurpoiqwurepqiurpqirpuqoiwrupqoirupqoirupqoiurpqiorupwqiourpqurpqurpqurpqurpqurpqurüqurqpuriq",
+)
```
I also feel like I'm misunderstanding what `Some(Parentheses)` implies.

---

_Label `formatter` added by @konstin on 2023-07-16 18:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/comprehension.rs`:118 on 2023-07-18 09:15_

`write!` has slightly more overhead and it depends on the compiler if it is able to *reduce* it exactly to `other.format().fmt(f)`

The difference are:

* The `write!` macro wraps the `[...]` in an `Arguments` struct, and each entry in the array in an `Argument` (it calls the `format_args![]` macro internally
* `write` calls into `buffer.write_fmt` which iterates over all arguments https://github.com/astral-sh/ruff/blob/df9e64dba0f9e5f2dadf85c543bfa548dabd42ae/crates/ruff_formatter/src/lib.rs#L717-L724
* `write` accepts any object that implements `Buffer` as the first argument. So you can do
  ```rust
	let mut recording = f.start_recording();
	write!(recording, [...])?;
	let recorded = recording.stop();
  ```
  whereas `.fmt(f)` directly uses the passed `Formatter`.


I tend to use `.fmt` for single items, and `write![]` for sequences. The downside of this is that adding a new item requires changing the code from `fmt` to `write` (or the other way round) and I don't do this consistently. That's why I'm okay leaving this to individuals to decide what they prefer.
	

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:18 on 2023-07-18 09:17_

How does `Comprehension` fit into your other PR where you tried to generalize the parentheses rule names (would this match `Never`)? Is it just comprehension where tuples aren't wrapped even if they exceed the line width? 

---

_@MichaReiser reviewed on 2023-07-18 09:18_

Nice. This looks good to me. My main question is about how this fits into your other work on parenthesizing tuples. 

---

_@cnpryer reviewed on 2023-07-18 13:10_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:18 on 2023-07-18 13:10_

This PR predates that PR. I wasn't sure if `Comprehension` matched the pattern that was already materializing or there was room to improve. I decided to propose an alternative that I'll try to shortly explain motivations behind below.

> Is it just comprehension where tuples aren't wrapped even if they exceed the line width?

I kinda wanna answer this with another question. From what perspective is `TupleParentheses` useful?

To me it's useful in defining formatting behavior for a tuple's parentheses. So if I'm implementing `Comprehension` I'd want to say "tuples here don't have parentheses" I'd probably use something closer to `TupleParentheses::Never`.

But to add to this, I'm still curious of understanding `NeedsParentheses`' role in all of this.

Is there a way to incorporate some of this configuration in how `NeedsParentheses` is implemented?

```rs
impl NeedsParentheses for ExprTuple {
    fn needs_parentheses(
        &self,
        parent: AnyNodeRef,
        _context: &PyFormatContext,
    ) -> OptionalParentheses {
        if parent.is_comprehension() {
            ...
        }
    }
}
```

Does this imply that if `needs_parentheses` is invoked somewhere we could just handle it with knowledge of the tuple's `parent`?

Is this where `TupleParentheses::Expr(Parentheses::Never)` becomes useful?

---

_@cnpryer reviewed on 2023-07-19 01:15_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/other/comprehension.rs`:118 on 2023-07-19 01:15_

48af71ad23e71b9d6cd6e40c0526cb5ef1fadc01

---

_@cnpryer reviewed on 2023-07-19 01:15_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:131 on 2023-07-19 01:15_

48af71ad23e71b9d6cd6e40c0526cb5ef1fadc01

---

_@cnpryer reviewed on 2023-07-19 01:17_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:18 on 2023-07-19 01:17_

What do you think about merging with `TupleParentheses::Comprehension` here and then I can better document motivations in #5810. 

---

_@cnpryer reviewed on 2023-07-19 02:02_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:17 on 2023-07-19 02:02_

https://github.com/astral-sh/ruff/pull/5810#discussion_r1267439876

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:18 on 2023-07-19 06:31_

> But to add to this, I'm still curious of understanding NeedsParentheses' role in all of this.

Okay, let's see if I can explain this well. 

It is possible to parenthesize an expression in Python (and most other languages). Adding parentheses allows to express the precedence explicitly. All these are parenthesized expressions: `(a)`, `(1), `([a, b])`.

Tuples are special because they have their own parentheses (like lists or dicts too), but they also happen to be the same as for parenthesizing expressions. The tuple parentheses are, in some situations, optional: so you can write `a, b` or `(a, b)`. The tuple `(a, b)` is a tuple with its optional parentheses, but it is not a parenthesized expression. It is only a parenthesized expression if you add one extra pair of parentheses: `((a, b))`. 

The logic in `FormatExpr` and `NeedsParantheses` is only concerned about the "generic" parentheses (parenthesized expressions). That's why `TupleParentheses` is the right approach in my view, because it doesn't control whether the expression should be parenthesized, but whether the tuple should preserve its parentheses. 

> Is this where TupleParentheses::Expr(Parentheses::Never) becomes useful?

I don't think this variant is used anymore and we should not use it to determine whether to keep the tuple parentheses (except if we **always** must preserve them if the expression is parenthesized). I must have overlooked it by one of my refactors that removed the `Parenthesize::Custom` variant.

> What do you think about merging with TupleParentheses::Comprehension here and then I can better document motivations in #5810.

Sounds good to me. Can you remove the `TODO Never` before merging?

---

_@MichaReiser approved on 2023-07-19 06:31_

---

_@cnpryer reviewed on 2023-07-19 12:01_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:18 on 2023-07-19 12:01_

This is so awesome. Thank you for all the detail. Happy to curate some docs on it if I can too.

3ddff0b9f51beaf88cd1b3907cc4c82f98c5d908

---

_Merged by @MichaReiser on 2023-07-19 12:05_

---

_Closed by @MichaReiser on 2023-07-19 12:05_

---

_Branch deleted on 2023-07-19 12:08_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:18 on 2023-07-19 22:52_

> removed the `Parentheses::Custom`

#5708 right? I'm checking that out now.

---

_@cnpryer reviewed on 2023-07-19 22:52_

---

_@MichaReiser reviewed on 2023-07-20 06:47_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_tuple.rs`:18 on 2023-07-20 06:47_

Yes, that's the PR that I had in mind but it didn't change `TupleParentheses::Expr` (or any logic related to it). It seems, that the `Expr` variant could have been unified with the `Default` variant from the beginning.

---
