```yaml
number: 12754
title: Consider deprecating UP027 or improve its docs
type: issue
state: closed
author: sk-
labels:
  - rule
assignees: []
created_at: 2024-08-08T14:10:13Z
updated_at: 2024-08-13T15:56:53Z
url: https://github.com/astral-sh/ruff/issues/12754
synced_at: 2026-01-10T11:09:54Z
```

# Consider deprecating UP027 or improve its docs

---

_Issue opened by @sk- on 2024-08-08 14:10_

The original rule [UP027 (unpacked-list-comprehension)](url), implemented in `pyupgrade` does not seem to be thoroughly justified, and in fact it is actually slower. Which goes against the documentation in ruff which mentions

> which is more efficient as it avoids allocating an intermediary list

I raised a question in [pyupgrade](https://github.com/asottile/pyupgrade/issues/961), showing how using generators is actually 3 times slower than unpacking a list comprehension

**System**
Python 3.12.4 (main, Jun  6 2024, 18:26:44) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Mac OS Sonoma 14.5

```
$ python -m timeit "a,b = [i for i in range(2)]"
5000000 loops, best of 5: 78.3 nsec per loop
$ python -m timeit "a,b,c = [i for i in range(3)]"
5000000 loops, best of 5: 86.2 nsec per loop
$ python -m timeit "a,b,c,d = [i for i in range(4)]"
5000000 loops, best of 5: 92.7 nsec per loop
$ python -m timeit "a,b,c,d,e = [i for i in range(5)]"
2000000 loops, best of 5: 114 nsec per loop

$ python -m timeit "a,b = (i for i in range(2))"
1000000 loops, best of 5: 225 nsec per loop
$ python -m timeit "a,b,c = (i for i in range(3))"
1000000 loops, best of 5: 263 nsec per loop
$python -m timeit "a,b,c,d = (i for i in range(4))"
1000000 loops, best of 5: 286 nsec per loop
$ python -m timeit "a,b,c,d,e = (i for i in range(5))"
1000000 loops, best of 5: 321 nsec per loop
```



---

_Comment by @charliermarsh on 2024-08-09 01:28_

@carljm -- Do you have an opinion here?

---

_Label `rule` added by @charliermarsh on 2024-08-09 01:28_

---

_Label `needs-decision` added by @charliermarsh on 2024-08-09 01:28_

---

_Comment by @trim21 on 2024-08-09 01:50_

should we have a reversed version as perf rule?

---

_Comment by @shoucandanghehe on 2024-08-09 05:49_

Hi guys, I did some small tests and the results show that in python 3.6-3.12 list comprehension are faster than generator

| Python Version | Elements | List Comprehension (nsec per loop) | Generator (nsec per loop) | Difference (nsec) |
|----------------|----------|------------------------------------|---------------------------|-------------------|
| 3.6.13         | 2        | 315 nsec                           | 364 nsec                  | +49 nsec          |
|                | 3        | 339 nsec                           | 402 nsec                  | +63 nsec          |
|                | 4        | 368 nsec                           | 438 nsec                  | +70 nsec          |
|                | 5        | 407 nsec                           | 475 nsec                  | +68 nsec          |
| 3.7.16         | 2        | 287 nsec                           | 350 nsec                  | +63 nsec          |
|                | 3        | 321 nsec                           | 387 nsec                  | +66 nsec          |
|                | 4        | 341 nsec                           | 412 nsec                  | +71 nsec          |
|                | 5        | 374 nsec                           | 455 nsec                  | +81 nsec          |
| 3.8.19         | 2        | 269 nsec                           | 321 nsec                  | +52 nsec          |
|                | 3        | 298 nsec                           | 356 nsec                  | +58 nsec          |
|                | 4        | 324 nsec                           | 391 nsec                  | +67 nsec          |
|                | 5        | 356 nsec                           | 423 nsec                  | +67 nsec          |
| 3.9.19         | 2        | 254 nsec                           | 296 nsec                  | +42 nsec          |
|                | 3        | 287 nsec                           | 328 nsec                  | +41 nsec          |
|                | 4        | 304 nsec                           | 358 nsec                  | +54 nsec          |
|                | 5        | 345 nsec                           | 385 nsec                  | +40 nsec          |
| 3.10.14        | 2        | 272 nsec                           | 317 nsec                  | +45 nsec          |
|                | 3        | 296 nsec                           | 353 nsec                  | +57 nsec          |
|                | 4        | 327 nsec                           | 382 nsec                  | +55 nsec          |
|                | 5        | 356 nsec                           | 408 nsec                  | +52 nsec          |
| 3.11.9         | 2        | 240 nsec                           | 276 nsec                  | +36 nsec          |
|                | 3        | 264 nsec                           | 301 nsec                  | +37 nsec          |
|                | 4        | 278 nsec                           | 335 nsec                  | +57 nsec          |
|                | 5        | 320 nsec                           | 351 nsec                  | +31 nsec          |
| 3.12.4         | 2        | 112 nsec                           | 200 nsec                  | +88 nsec          |
|                | 3        | 122 nsec                           | 219 nsec                  | +97 nsec          |
|                | 4        | 136 nsec                           | 235 nsec                  | +99 nsec          |
|                | 5        | 155 nsec                           | 254 nsec                  | +99 nsec          |

Maybe we should remove UP027 and implement a reverse version of it.

---

_Comment by @carljm on 2024-08-09 05:51_

I agree that the current `UP027` doesn't make any sense. Tbh I'm not a big fan of the rule in either direction; the perf difference is not massive and won't matter for most code (and may change in future as faster-cpython team have plans to improve generator perf), and I don't think there's a strong stylistic argument in either direction either. But if it makes sense to have the rule at all, it would make sense to have it in the other direction (to prefer list comprehension.)

---

_Comment by @MichaReiser on 2024-08-09 15:59_

@AlexWaygood should we deprecate this rule as part of 0.6?

---

_Comment by @Avasam on 2024-08-12 05:33_

On the note of an inverse rule, I think that's basically what I'm asking in #11839 ?

---

_Added to milestone `v0.6` by @MichaReiser on 2024-08-12 06:36_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-08-12 10:42_

---

_Comment by @AlexWaygood on 2024-08-12 11:26_

> @AlexWaygood should we deprecate this rule as part of 0.6?

Yeah, makes sense to me. I'd hold off with implementing a reverse rule, though; I agree with @carljm that there doesn't seem to be a strong argument for preferring either

---

_Comment by @MichaReiser on 2024-08-12 11:56_

There are cases where pure generators are faster, but only when they're large enough which is way beyond the size someone would unpack ;)

```
ruff on  ruff-0.6 [$!] is 📦 v0.5.7 via 🐍 v3.12.4 via 🦀 v1.80.0 took 2s 
❯ python -m timeit "a = sum([i for i in range(1_000_000)])"
10 loops, best of 5: 31.3 msec per loop

ruff on  ruff-0.6 [$!] is 📦 v0.5.7 via 🐍 v3.12.4 via 🦀 v1.80.0 took 2s 
❯ python -m timeit "a = sum((i for i in range(1_000_000)))"
10 loops, best of 5: 27 msec per loop

❯ python -m timeit "a = sum((i for i in range(10_000_000)))"
1 loop, best of 5: 288 msec per loop

ruff on  ruff-0.6 [$!] is 📦 v0.5.7 via 🐍 v3.12.4 via 🦀 v1.80.0 
❯ python -m timeit "a = sum([i for i in range(10_000_000)])"
1 loop, best of 5: 357 msec per loop

```



---

_Label `needs-decision` removed by @MichaReiser on 2024-08-12 11:58_

---

_Closed by @MichaReiser on 2024-08-13 15:56_

---
