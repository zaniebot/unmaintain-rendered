```yaml
number: 22628
title: "New rule: iterator use after exhaustion"
type: issue
state: open
author: GideonBear
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2026-01-16T18:29:59Z
updated_at: 2026-01-16T18:48:33Z
url: https://github.com/astral-sh/ruff/issues/22628
synced_at: 2026-01-16T19:01:35Z
```

# New rule: iterator use after exhaustion

---

_@GideonBear_

### Summary

Forgetting to use `list(map(` bit me today, and it occurred to me that this is statically findable!

## Rule suggestion

When an iterator is assigned to a variable, and that iterator is used exhaustively, any further use of that iterator (use of that variable without re-assigning) is disallowed, as the iterator will be empty.

In addition:
* Any exhaustive use in a loop is definitely wrong
* Any use in a loop is probably wrong (like [reuse-of-groupby-generator (B031)](https://docs.astral.sh/ruff/rules/reuse-of-groupby-generator/#reuse-of-groupby-generator-b031) flags)

Examples of iterators:
* Calls to `iter`, `enumerate`, `map`, `filter`, `zip`, `reversed`
* In the future when type-awareness is a thing, calls to any function or generator that returns an `Iterator`
* Generator expressions
* ...?

Examples of exhaustive use:
* `list(it)`, probably some other types and builtins as well
* Unsound possibility: passing it to any function
  * 9/10 times the iterator is exhausted immediately in the function. Would like to see a primer on this. A function taking and not exhausting an iterator seems really rare.
* `for x in it` without `break`s
  * `return`s are allowed as the iterator is dropped after returning
  * `raise`s are allowed unless the loop is located in a `try` block and the iterator is assigned outside the `try` block
* ...?
* List/set/dict comprehension
* Unsound possibility: generator expressions
  * Not completely sound, since you could technically do something like:
```py
it = iter(l)
gx = (foo for el in it)
print(next(it))
print(list(gx))
```
But then you could (and should) define `gx` after `print(next(it))`, so I still think this should be considered as exhausting the iterator. Especially since we want to catch `f(el for el in it)`, just like with "passing it to any function".

## Examples
```py
it = iter(l)
for el in it:
    print(el)
for el in it:
    print(el)

it = iter(l)
for el in it:
    print(el)
print(next(it))

# This was mine
it = iter(l)
for el in it:
    print(el)
[el for el in it]
```


---

_Comment by @ntBre on 2026-01-16 18:39_

Thanks for the suggestion! I have personally been bitten by this in the past. We have a rule for one specific instance of this in [reuse-of-groupby-generator (B031)](https://docs.astral.sh/ruff/rules/reuse-of-groupby-generator/#reuse-of-groupby-generator-b031) but something more general would be helpful.

I'll put needs-decision on this for now to let others weigh in, but I think this would be a useful rule.

---

_Label `rule` added by @ntBre on 2026-01-16 18:39_

---

_Label `needs-decision` added by @ntBre on 2026-01-16 18:39_

---

_Comment by @GideonBear on 2026-01-16 18:48_

FWIW a simple not-type-aware version of this lint would not flag [reuse-of-groupby-generator (B031)](https://docs.astral.sh/ruff/rules/reuse-of-groupby-generator/#reuse-of-groupby-generator-b031). I've also broadened my suggestion a bit according to what B031 does.

---
