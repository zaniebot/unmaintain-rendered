---
number: 18849
title: PLW3301 false positive if outer call has multiple arguments and inner call has one argument
type: issue
state: closed
author: zackw
labels:
  - documentation
  - rule
assignees: []
created_at: 2025-06-21T14:22:33Z
updated_at: 2025-06-27T14:29:11Z
url: https://github.com/astral-sh/ruff/issues/18849
synced_at: 2026-01-07T13:12:16-06:00
---

# PLW3301 false positive if outer call has multiple arguments and inner call has one argument

---

_Issue opened by @zackw on 2025-06-21 14:22_

### Summary

This is the converse of #16163.  In general,

```
    v1 = max(lower_bound, max(iterable_1)) # erroneous PLW3301
    v2 = min(upper_bound, min(iterable_2)) # erroneous PLW3301
```

cannot be expressed any other way, but -- even after the change in #16885 -- ruff with PLW3301 active will tell you to change it to

```
    v1 = max(lower_bound, iterable_1) # now crashes
    v2 = min(upper_bound, iterable_2) # now crashes
```

which is probably going to throw a TypeError, and even if it doesn't, it won't do the same thing as the original.

---

_Comment by @MichaReiser on 2025-06-23 07:38_

Can you tell me what Ruff version you're using and how `lower_bound` and `iterable_1` are defined?

The `v1` example gets fixed to `max(lower_bound, *iterable_1)` which seems correct to me. 

`v2` gets fixed to `min(upper_bound, *iterable_2)` which also seems correct to me

---

_Label `rule` added by @MichaReiser on 2025-06-23 07:38_

---

_Label `needs-info` added by @MichaReiser on 2025-06-23 07:38_

---

_Comment by @zackw on 2025-06-24 14:00_

I was looking at the playground, which doesn't show any fixes for PLW3301.  Maybe they're off by default, but I couldn't figure out how to turn them on.  See for example <https://play.ruff.rs/2820f381-fadb-444d-902d-811ac928f178>.

Regardless, the suggested fix is bad, because it forces Python to allocate a tuple for the entire contents of the iterable.  Compare:

```sh
$ python3.13 -m timeit -- 'max(12345, max(range(10_000_000)))'
1 loop, best of 5: 475 msec per loop
$ python3.13 -m timeit -- 'max(12345, *range(10_000_000))'
1 loop, best of 5: 652 msec per loop
```

In realistic (rather than microbenchmark) situations the memory overhead of the splat may also be unacceptable.

---

_Comment by @ntBre on 2025-06-24 14:13_

This could possibly be a separate bug report, but PLW3301 doesn't offer a fix if there are comments in the replacement range. We usually just mark the fix as unsafe in those cases now. This playground will allow fixes to be applied: https://play.ruff.rs/7e017746-8622-48fc-b828-318056e67065

https://github.com/astral-sh/ruff/blob/02ae8e1210b074dfeb32e748850aeb0d892735d3/crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs#L179-L183

---

_Comment by @MichaReiser on 2025-06-25 07:02_

I see. This isn't about a false positive in the sense that Ruff's suggested fix is incorrect or breaks program semantics but because it can degrade performance in micro benchmarks. I feel like this is a good use case of a noqa, given how subtle the performance difference is (it's unlikely to matter in 99% of cases). We could change our documentation, though.

@ntBre nice find. Do you want to put a PR up for this. It should be a very quick change.

---

_Label `needs-info` removed by @MichaReiser on 2025-06-25 07:02_

---

_Label `documentation` added by @MichaReiser on 2025-06-25 07:02_

---

_Referenced in [astral-sh/ruff#18936](../../astral-sh/ruff/pulls/18936.md) on 2025-06-25 13:19_

---

_Closed by @ntBre on 2025-06-25 13:29_

---

_Comment by @zackw on 2025-06-27 14:04_

We must be coming at this from very different premises.  To me, there is almost no readability difference between `max(bound, max(iterable))` and `max(bound, *iterable)`, and therefore the performance advantage of the nested `max` call, small as it is in most cases, is sufficient reason to say that this _is_ a genuine false positive: Ruff should not be making this suggestion at all.

But evidently you think that `max(bound, *iterable)` is _better_ than `max(bound, max(iterable))` for some reason, sufficient to relegate the performance cost to the land of "mark it noqa when it matters". Please help me understand why you think this. Do you find `max(bound, *iterable)` significantly more readable? Is it something else I haven't mentioned?

---

_Comment by @ntBre on 2025-06-27 14:29_

The rule is called `nested-min-max`, and there's a nested `max` call in `max(bound, max(iterable))`, so I think it's pretty hard to argue that this is a false positive. It's also flagged by the upstream linter, although the error message isn't quite correct:

```shell
$ cat try.py
max(1, max(range(3)))
$ pylint try.py
************* Module try
try.py:1:0: C0114: Missing module docstring (missing-module-docstring)
try.py:1:0: W3301: Do not use nested call of 'max'; it's possible to do 'max(1, range(3))' instead (nested-min-max)

------------------------------------------------------------------
Your code has been rated at 0.00/10 (previous run: 0.00/10, +0.00)
```

And I do find it more readable, personally. In the nested case, I have to stop and consider the associativity of `max`, whereas in a single call, it's pretty clearly going to be the max of the whole collection.

---
