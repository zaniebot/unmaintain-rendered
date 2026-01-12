```yaml
number: 5073
title: Warn against quadratic list summation
type: issue
state: closed
author: hauntsaninja
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-06-14T03:40:58Z
updated_at: 2024-02-26T00:38:27Z
url: https://github.com/astral-sh/ruff/issues/5073
synced_at: 2026-01-12T15:54:45Z
```

# Warn against quadratic list summation

---

_@hauntsaninja_

I can't find a linter that warns me about `sum(list_of_lists, [])`. Basically any time the `default` arg of `sum` is a list literal, I think something slow could happen. Maybe ruff could warn me about this? :-)

---

_Comment by @konstin on 2023-06-14 10:12_

Is the correct replacement for this `list(itertools.chain(*list_of_lists))` or is there a better solution? I've been following https://stackoverflow.com/a/953097/3549270 so far

---

_Label `rule` added by @charliermarsh on 2023-06-14 14:23_

---

_Comment by @hauntsaninja on 2023-06-14 19:09_

Yup, that'll work!

I don't have strong opinions on the replacement, they all seem to be a constant factor off.
- `list(itertools.chain.from_iterable(list_of_lists))` looks to be a shade faster than `list(itertools.chain(*list_of_lists))` on my machine
- Looking at your StackOverflow link I see mention of `functools.reduce(operator.iadd, list_of_lists, [])`, which is fastest for me
- The most straightforward is probably just `[x for lst in list_of_lists for x in lst]`

```
λ python3.11 -m timeit -n 10000 -r 20 -s 'l=[[1,2,3],[4,5,6], [7], [8,9]]*99;import itertools,functools,operator' '[x for y in l for x in y]'
10000 loops, best of 20: 25.5 usec per loop
λ python3.11 -m timeit -n 10000 -r 20 -s 'l=[[1,2,3],[4,5,6], [7], [8,9]]*99;import itertools,functools,operator' 'list(itertools.chain(*l))'
10000 loops, best of 20: 18.5 usec per loop
λ python3.11 -m timeit -n 10000 -r 20 -s 'l=[[1,2,3],[4,5,6], [7], [8,9]]*99;import itertools,functools,operator' 'list(itertools.chain.from_iterable(l))'
10000 loops, best of 20: 17.5 usec per loop
λ python3.11 -m timeit -n 10000 -r 20 -s 'l=[[1,2,3],[4,5,6], [7], [8,9]]*99;import itertools,functools,operator' 'functools.reduce(operator.iadd, l, [])'
10000 loops, best of 20: 11.7 usec per loop
```

Repro using the nice perfplot code from your StackOverflow link:

![out](https://github.com/astral-sh/ruff/assets/12621235/502fc680-1430-4975-89c3-097a4e23de5d)


---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:27_

---

_Comment by @hauntsaninja on 2023-07-25 22:36_

Saw a "needs decision" label got added. I'd vote the best replacement is `functools.reduce(operator.iadd, l, [])`. If we don't want to add imports, then `[x for y in l for x in y]`.

---

_Comment by @charliermarsh on 2023-07-25 23:00_

I'm cool with adding this as long as the ecosystem checks look okay (that is: no major false positives).

---

_Label `needs-decision` removed by @charliermarsh on 2023-07-25 23:00_

---

_Label `accepted` added by @charliermarsh on 2023-07-25 23:00_

---

_Comment by @evanrittenhouse on 2023-08-06 04:05_

You can assign this to me. I should have a fix out soon.

---

_Assigned to @evanrittenhouse by @dhruvmanila on 2023-08-06 07:08_

---

_Closed by @charliermarsh on 2023-08-17 03:13_

---

_Comment by @arogozhnikov on 2024-02-25 10:19_

funny enough, if I use `sum(l, [])`, it is slow

But when I reimplement the same code in a very naive way:
```
def sum_lists(x):
    res = []
    for i in x:
        res += i
    return res
```
it is on par with proposed solutions (or even faster)

---

_Comment by @hauntsaninja on 2024-02-26 00:38_

Yes, that's the magic of `iadd` instead of `add`.

---
