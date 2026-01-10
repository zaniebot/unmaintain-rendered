```yaml
number: 21879
title: Fix leading comment formatting for lambdas with multiple parameters
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: brent/fix-kwargs
created_at: 2025-12-09T20:40:46Z
updated_at: 2025-12-09T23:15:14Z
url: https://github.com/astral-sh/ruff/pull/21879
synced_at: 2026-01-10T16:42:11Z
```

# Fix leading comment formatting for lambdas with multiple parameters

---

_Pull request opened by @ntBre on 2025-12-09 20:40_

## Summary

This is a follow-up to #21868. As soon as I started merging #21868 into #21385, I realized that I had missed a test case with `**kwargs` after the `*args` parameter. Such a case is supposed to be formatted on one line like:

```py
# input
(
    lambda
    # comment
    *x,
    **y: x
)

# output
(
    lambda
    # comment
    *x, **y: x
)
```

which you can still see on the [playground](https://play.ruff.rs/bd88d339-1358-40d2-819f-865bfcb23aef?secondary=Format), but on `main` after #21868, this was formatted as:

```py
(
    lambda
    # comment
    *x,
    **y: x
)
```

because the leading comment on the first parameter caused the whole group around the parameters to break.

Instead of making these comments leading comments on the first parameter, this PR makes them leading comments on the parameters list as a whole.

## Test Plan

New tests, and I will also try merging this into #21385 _before_ opening it for review this time.

<hr>

(labeling `internal` since #21868 should not be released before some kind of fix)


---

_Label `internal` added by @ntBre on 2025-12-09 20:40_

---

_Label `formatter` added by @ntBre on 2025-12-09 20:40_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 20:52_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @MichaReiser on 2025-12-09 20:57_

> , but this caused some trouble in the formatting of function parameters

Can you say more about this? It seems strictly better to me than poking into the parameter comments. Can't we detect the lambda case by looking at the parent node in the comment placement logic or whether `parameters` has parentheses?

---

_Comment by @ntBre on 2025-12-09 21:13_

It inserted a comment before the parentheses in a function definition, but maybe I just needed to check if it had parentheses.

The other issue is that we also have to make this case leading on the parameters to get consistent formatting:

```py
(
    lambda
    * # comment
    x, **y:
    x
)
```

which means we'd have to modify the parameter comment placement and need the grandparent node again, instead of only modifying the lambda placement like in #21868. Otherwise we're back to a similar situation as before #21868 where this is a leading comment on the first parameter on the first format, causing the parameters to break, and a leading comment on the parameters on the second, allowing them to stay on one line.

That's the other reason I switched to this approach.

---

_Comment by @MichaReiser on 2025-12-09 21:27_

> which means we'd have to modify the parameter comment placement and need the grandparent node again, instead of only modifying the lambda placement like in https://github.com/astral-sh/ruff/pull/21868

Why would we need the grand parent? Isn't the parent `parameters`?

---

_Comment by @ntBre on 2025-12-09 21:29_

> Why would we need the grand parent? Isn't the parent parameters?

Yes, but we need to know if the parameters are in a lambda, or we'll break functions. At least that was my thinking and what I believe I ran into earlier. But I could have missed something else, of course.

---

_Comment by @MichaReiser on 2025-12-09 21:30_

> Yes, but we need to know if the parameters are in a lambda, or we'll break functions. At least that was my thinking and what I believe I ran into earlier. But I could have missed something else, of course.

We can tell whether we're inside a lambda by looking if the parameters are parenthesized.

---

_Comment by @ntBre on 2025-12-09 21:31_

Ah okay, right, like in your first comment. Sorry, I'll give that a try!

---

_Comment by @MichaReiser on 2025-12-09 21:41_

I think the way you can dedect this is by testing if `parameters.start()` == `parameter.start()`

---

_Comment by @ntBre on 2025-12-09 21:45_

There's actually already a `are_parameters_parenthesized` helper in `placement.rs` that seems to work :+1: 

Apparently the code I stashed earlier was working except for that check. Thanks again!

---

_@MichaReiser approved on 2025-12-09 22:30_

---

_Marked ready for review by @ntBre on 2025-12-09 22:30_

---

_Merged by @ntBre on 2025-12-09 23:15_

---

_Closed by @ntBre on 2025-12-09 23:15_

---

_Branch deleted on 2025-12-09 23:15_

---
