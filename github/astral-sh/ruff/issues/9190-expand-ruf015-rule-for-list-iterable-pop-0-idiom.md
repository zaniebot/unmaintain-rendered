---
number: 9190
title: "Expand `RUF015` rule for `list(iterable).pop(0)` idiom"
type: issue
state: closed
author: Skylion007
labels:
  - good first issue
  - rule
  - accepted
assignees: []
created_at: 2023-12-18T18:19:39Z
updated_at: 2024-02-29T02:39:17Z
url: https://github.com/astral-sh/ruff/issues/9190
synced_at: 2026-01-07T13:12:15-06:00
---

# Expand `RUF015` rule for `list(iterable).pop(0)` idiom

---

_Issue opened by @Skylion007 on 2023-12-18 18:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I just saw another method of accessing the first element of an iterable / dictionary key that is very inefficient. Currently, this is not detected by RUF015 which we have enabled. We should expand RUF015 to cover this idiom. A minimal problematic example is found below.
```python
first_element = list(iterable).pop(0)
```
It should be
```python
first_element = next(iter(iterable))
```
which would not require iterating through the entire iterable just to retrieve the first element. Expanding the autofix to cover this would be helpful as well.
* RUFF 0.1.8 (and all earlier versions are affected).

---

_Renamed from "Expand `RUF015` rule for list(iterable).pop() idiom" to "Expand `RUF015` rule for `list(iterable).pop()` idiom" by @Skylion007 on 2023-12-18 18:21_

---

_Label `rule` added by @charliermarsh on 2023-12-18 19:30_

---

_Label `accepted` added by @charliermarsh on 2023-12-18 19:30_

---

_Comment by @charliermarsh on 2023-12-18 19:30_

👍 Makes sense!

---

_Comment by @T-256 on 2023-12-19 13:47_

```py
>>> iterable = [1,2]
>>> list(iterable).pop()
2
>>> next(iter(iterable))
1
>>> 
```

---

_Comment by @charliermarsh on 2023-12-19 14:44_

Oh, oops, the `.pop()` takes the last item, not the first. We could wrap in a `reversed`...

---

_Comment by @Skylion007 on 2023-12-19 16:03_

Oh whoops. Good catch, I misremembered the default arg of pop. This still applies to `pop(0)` though. The reversed iter next isn't bad though. I wonder if a deque could be abused to grab the last element too.

---

_Renamed from "Expand `RUF015` rule for `list(iterable).pop()` idiom" to "Expand `RUF015` rule for `list(iterable).pop(0)` idiom" by @Skylion007 on 2023-12-19 16:04_

---

_Comment by @UnknownPlatypus on 2023-12-19 17:17_

Using `reversed` instead of `iter` in the `.pop()` case is great no? (no need to `reversed(iter(...))` I think?)
```diff
-list(iterable).pop(0)
+next(iter(iterable))

-list(iterable).pop()
+next(reversed(iterable))
```


---

_Comment by @Skylion007 on 2023-12-19 17:31_

Oh, great point!

---

_Comment by @Skylion007 on 2023-12-19 17:35_

~Actually, one concern if the generator is very, very long and if the iterable doesn't implement `__reversed__`, then the `next(reversed(iterable))` could take a lot of memory, right? I think~ the most efficient solution is to use reversed if it does implement this method, otherwise use `deque(iterable, max_len=1).pop()`, right? `reversed` only works on sequences not iterators so I am not sure it's a drop in replacement sadly.

```python
a = iter(range(10))
b = reversed(a)
```
will return an error.

---

_Comment by @UnknownPlatypus on 2023-12-19 18:14_

oh right, this is a tricky one. However, I'm not sure `deque(iterable, max_len=1).pop()` will actually be faster. On my machine it's actually slower. The iterable is probably evaluated entirely idk:
```python
%timeit deque(range(10000), maxlen=1).pop()
88.7 µs ± 8.44 µs per loop (mean ± std. dev. of 7 runs, 10,000 loops each)
%timeit list(range(10000)).pop()
73.8 µs ± 252 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)
```

---

_Comment by @Skylion007 on 2023-12-19 20:20_

Yeah, this is more about memory management than speed. I think we agree that the`pop(0)` change is definitely not controversial though.

---

_Label `good first issue` added by @charliermarsh on 2024-02-07 22:48_

---

_Comment by @hauntsaninja on 2024-02-08 09:33_

(this should be an unsafe fix)

---

_Comment by @rmad17 on 2024-02-14 09:18_

I am a beginner in Rust and would like to contribute. Can I take this up? 
Going through the comments I got some idea but its not clear what is the solution.

---

_Comment by @dhruvmanila on 2024-02-15 04:59_

> I am a beginner in Rust and would like to contribute. Can I take this up? 

Sure!

> Going through the comments I got some idea but its not clear what is the solution.

So, currently the [implementation takes in an `ExprSubscript`](https://github.com/astral-sh/ruff/blob/4c62227da2ecbc6fe7b069b633ac3a233e9100fc/crates/ruff_linter/src/rules/ruff/rules/unnecessary_iterable_allocation_for_first_element.rs#L68-L71) which matches the `list(iterable)[0]` code ([AST Playground](https://play.ruff.rs/a06b12d6-2c6c-41c1-a308-278eb3257191)). What we want is to also look for code like `list(iterable).pop(0)` which is an `ExprAttribute` ([AST Playground](https://play.ruff.rs/262f86d3-a469-4a3a-ae42-ce6b5cdfd55b)).

This will require updating the function signature to now take any `Expr` to allow for either `Expr::Subscript` or `Expr::Attribute`. You'll need to update the function body to check for the `.pop(0)` idiom along with the existing `[0]` (`is_head_slice`). Rest of the code should probably be the same.

The fix for this rule is already unsafe and it would help expand on the [fix safety](https://docs.astral.sh/ruff/rules/unnecessary-iterable-allocation-for-first-element/#fix-safety) documentation of the rule with why this fix is unsafe w.r.t. the new code.

Hope this helps, feel free to ask any other questions that you might have. I'll assign this issue to you but don't feel any pressure to complete this :)



---

_Assigned to @rmad17 by @dhruvmanila on 2024-02-15 05:00_

---

_Referenced in [astral-sh/ruff#10148](../../astral-sh/ruff/pulls/10148.md) on 2024-02-27 22:21_

---

_Comment by @robincaloudis on 2024-02-27 22:36_

Since there was no activity on this issue yet (followed it for the last 2 weeks), I stepped ahead and provided a fix in https://github.com/astral-sh/ruff/pull/10148. I hope I interpreted the inactivity on this ticket correctly and didn't steal someones chance. @dhruvmanila, @charliermarsh, do you mind reviewing the PR? Thanks a lot.

---

_Closed by @charliermarsh on 2024-02-28 00:24_

---

_Comment by @rmad17 on 2024-02-29 02:39_

@robincaloudis Thanks for taking it up, I was not able to look into it despite asking for the ticket to be assigned. 

---
