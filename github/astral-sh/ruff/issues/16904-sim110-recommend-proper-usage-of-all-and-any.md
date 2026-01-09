---
number: 16904
title: "SIM110: Recommend proper usage of `all` and `any`"
type: issue
state: open
author: davfsa
labels:
  - documentation
  - rule
  - needs-decision
assignees: []
created_at: 2025-03-21T19:57:10Z
updated_at: 2025-03-31T18:32:16Z
url: https://github.com/astral-sh/ruff/issues/16904
synced_at: 2026-01-07T13:12:16-06:00
---

# SIM110: Recommend proper usage of `all` and `any`

---

_Issue opened by @davfsa on 2025-03-21 19:57_

### Summary

## The problem

`all` and `any` are great built-ins, but only when used correctly. They have common pitfalls that seriously hurts performance.

Taking for example, the following piece of code (this is *not* real code, it is just an example!):
```py
for elm in lst:
    if elm > 0:
        return True
return False
```

ruff (if rule SIM110 is enabled) will suggest to turn it into:
```py
return any(elm > 0 for elm in lst)
```

which uses a generator expression, which is wayy (2 y's) slower than the for loop it is replacing
<details>
<summary>Benchmark</summary>

```py
lst = [0 for _ in range(10000)] + [1]

def for_list():
    for elm in lst:
        if elm > 0:
            return True
    return False

def any_gen():
    return any(elm > 0 for elm in lst)

def any_proper_gen():
    return any(True for elm in lst if elm > 0)

def any_list():
    return any([elm > 0 for elm in lst])
```

</details>

```
%timeit for_list()
113 μs ± 253 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)

%timeit any_gen()
225 μs ± 1.67 μs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)

%timeit any_proper_gen()
108 μs ± 182 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)

%timeit any_list()
130 μs ± 531 ns per loop (mean ± std. dev. of 7 runs, 10,000 loops each)
```

*You usually want lazy evaluation in these cases, which is why I included `any_list` to show that `[]` could be another option, but is usually unwanted)* 

## Proposed solution

Instead of the original code, ruff should suggest:
```py
return any(True for elm in lst if elm > 0)
```

This is a bit more unreadable, so it would maybe be paired with another performance rule (probably under PERF) to catch incorrect uses of `any` and `all`, as well as suggest the correct code for SIM110

## References
I always assumed `any` and `all` were slower than a normal loop outright, until I started migrating one of my projects to use ruff, which lead me to actually research this and found these amazing answers on StackOverflow, which helped me greatly to write this issue

https://stackoverflow.com/a/44803103
https://stackoverflow.com/a/44813751

---

_Label `rule` added by @ntBre on 2025-03-21 20:41_

---

_Comment by @ntBre on 2025-03-21 20:50_

Hmm, I definitely agree with you that the proposed code is more unreadable, so I don't think it would be a great fit for the output of SIM110 itself. Maybe a separate `PERF` rule would be a better approach, like you said.

I think we try to avoid too much discussion of performance in the rule documentation, but it might be worth noting in this case too.

---

_Label `documentation` added by @ntBre on 2025-03-21 20:50_

---

_Comment by @davfsa on 2025-03-21 21:00_

> Hmm, I definitely agree with you that the proposed code is more unreadable, so I don't think it would be a great fit for the output of SIM110 itself. Maybe a separate PERF rule would be a better approach, like you said.

Sounds great! I wanted to get some confirmation before attempting to work on this.

Would like to also confirm if it would be ok for rules to be "dependent" on other rules. My idea would be to produce different output for SIM110 if the new PERF rule is enabled? This would prevent needing to re-run ruff if (for example) rule SIM110 gets applied with `--fix`

---

_Comment by @ntBre on 2025-03-21 21:09_

Let's wait a little while to see if there's more interest in this kind of rule. Adding new rules isn't a top priority for us right now. You can still work on it, of course, but we may be a bit slow in reviewing it 🙂 

Ruff applies fixes in a loop until they stabilize, which is how we usually handle this kind of interaction. In this case, SIM110 would apply, converting the `for` loop to the unoptimized `any` call, and then the hypothetical PERF rule would run to convert the unoptimized `any` into the optimized version. So I don't think you'd have to handle the dependency directly.

---

_Label `needs-decision` added by @ntBre on 2025-03-21 21:09_

---

_Comment by @davfsa on 2025-03-21 21:17_

Thanks for the answer!

And yeah, I dont expect this to be immediate and out in a release by tomorrow. I expected conversation to flow a bit before coming to a decision. Just wanted to leave a pull request a long with the issue (if possible) as a bit of an excuse to write a bit of rust code :)

---

_Renamed from "Recomment proper usage of `all` and `any`" to "SIM110: Recomment proper usage of `all` and `any`" by @MichaReiser on 2025-03-21 21:25_

---

_Renamed from "SIM110: Recomment proper usage of `all` and `any`" to "SIM110: Recommend proper usage of `all` and `any`" by @dylwil3 on 2025-03-21 21:56_

---

_Comment by @davfsa on 2025-03-21 22:00_

Further to note (after a bunch of testing). It seems like both `any` and `all` is really benefited (performance wise) a bit from the fix, compared to just writing out the loop. Im assuming that this is due to the double encapsulation of for loops (essentially, we are checking the truthfulness of the value twice). 

Im starting to think if its better for (in the name of small performance gains) to just advice to *not* use `all` and `any` and just write out the loop 🤔 

---

_Comment by @davfsa on 2025-03-31 15:46_

It looks like this might no longer be needed after CPython 3.14 https://github.com/python/cpython/pull/131737.

---

_Comment by @Avasam on 2025-03-31 18:18_

> It looks like this might no longer be needed after CPython 3.14 [python/cpython#131737](https://github.com/python/cpython/pull/131737).

Looks like that may affect https://github.com/astral-sh/ruff/issues/12912 as well? 👀 

---

_Referenced in [astral-sh/ruff#12912](../../astral-sh/ruff/issues/12912.md) on 2025-03-31 18:21_

---

_Comment by @Avasam on 2025-03-31 18:31_

I personally still prefer 

```py
any(True for elm in lst if elm > 0)
```
over
```py
for elm in lst:
    if elm > 0:
        return True
return False
```

Especially if it comes with a lint rule/autofix.

Like yeah someone may (rightfully) wonder why it's written that way, but as soon as they "simplify" the expression, Ruff will tell them why.

---

*something something readability, python isn't a performant language*.
But Python is widely supported across platforms for specific use-cases. And getting "free" (as in the linter does it for you) performance gains is nice in a language that needs all the help it can get. It's not a micro-optimisation if you wrote it right the first time 😉 

---

So consider this my vote for such a rule. Although we should really get some numbers on the Python 3.14 generator expression changes.

---

_Referenced in [astral-sh/ruff#19419](../../astral-sh/ruff/issues/19419.md) on 2025-07-28 21:51_

---
