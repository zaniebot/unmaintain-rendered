```yaml
number: 6534
title: False positives for B002 (unary-prefix-increment-decrement)
type: issue
state: open
author: deepyaman
labels:
  - needs-decision
assignees: []
created_at: 2023-08-13T12:08:16Z
updated_at: 2023-08-14T13:20:48Z
url: https://github.com/astral-sh/ruff/issues/6534
synced_at: 2026-01-10T11:09:48Z
```

# False positives for B002 (unary-prefix-increment-decrement)

---

_Issue opened by @deepyaman on 2023-08-13 12:08_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hi! I'm getting a B002 error on https://github.com/ibis-project/ibis/blob/6.1.0/ibis/tests/expr/test_table.py#L1243. I believe (without actually understanding how the Ruff implementation works) this is because the AST for `-(-(x))` is equivalent for `--x`, but:

1. the parentheses make it such that it's not actually an attempt at using non-existent unary prefix decrement operator
2. the error message is therefore misleading

I understand that this is an edge case, because most code doesn't define/test the unary `-`/`+` operator, and will therefore just stick `noqa`'s on the code, but thought it still worth reporting in case there's an opportunity to make the error a bit less confusing.

Ruff version: 0.0.284

---

_Comment by @charliermarsh on 2023-08-13 21:15_

Just want to make sure I understand: I think `-(-(t1.key1 == t2.key1).any())` _is_ equivalent to `--((t1.key1 == t2.key1).any())`, they produce the same AST due to operator precedence. So this _is_ a case in which the unary prefix decrement operator is being used. But are you saying that it's _not_ such a case, or that it's _intentional_ here and so shouldn't be flagged?

---

_Comment by @charliermarsh on 2023-08-13 21:15_

As always, thank you for reporting :)

---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-13 21:16_

---

_Comment by @deepyaman on 2023-08-14 05:15_

> Just want to make sure I understand: I think `-(-(t1.key1 == t2.key1).any())` _is_ equivalent to `--((t1.key1 == t2.key1).any())`, they produce the same AST due to operator precedence. So this _is_ a case in which the unary prefix decrement operator is being used. But are you saying that it's _not_ such a case, or that it's _intentional_ here and so shouldn't be flagged?

Hi! Yes, I also think it's producing the same AST, hence getting flagged (as I wrote in the original issue). However, since the unary prefix decrement operator isn't actually a thing in Python (which is the reason why the B002 rule exists), I'll be pedantic and say that `--((t1.key1 == t2.key1).any())` would technically be equivalent to what's more clearly written in the code I'm touching, `-(-(t1.key1 == t2.key1).any())`. `-(-(t1.key1 == t2.key1).any())` fairly indicates what the code is doing--it's testing negation of negation.

I think this is all a roundabout way of saying

> it's _intentional_ here and so shouldn't be flagged

---

_Comment by @deepyaman on 2023-08-14 05:25_

Since the above reply is a bit convoluted, maybe I can try to re-explain with a simpler example:

* If my code reads `--x`, it seems fair to raise B002, because it's very possible I'm coming from another language and will get behavior I don't expect (that it will return `x`, instead of decrementing `x`).
* If my code reads `-(-(x))`, on the other hand, it's pretty clear that I didn't just come from C++ and am trying to pre-decrement; it's something else. If I did want to pre-decrement, why the parentheses? So, it's a bit misleading to say, "Python does not support the unary prefix decrement operator" IMO; as the user, I never had any intention of pre-decrementing, and (if I didn't have prior experience outside of Python) I wouldn't even recognize `-(-(x))` as pre-decrementing.
* If it's not possible to provide distinguished error messages because the ASTs for `--x` and `-(-(x))` are equivalent (I'm not familiar with the Ruff codebase, so I wouldn't know whether it's possible or not), I understand. Like I said, I can get around it in my specific case with some `noqa`s, and I use cases for writing `-(-(x))` are probably few and far between (e.g. testing library code, which is the use case I saw).

---

_Label `waiting-on-author` removed by @charliermarsh on 2023-08-14 13:20_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-14 13:20_

---
