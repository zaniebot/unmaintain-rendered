```yaml
number: 21891
title: PERF401 extend suggestion has false negatives and makes code slower
type: issue
state: open
author: hauntsaninja
labels:
  - bug
  - rule
assignees: []
created_at: 2025-12-10T09:37:16Z
updated_at: 2026-01-05T21:11:04Z
url: https://github.com/astral-sh/ruff/issues/21891
synced_at: 2026-01-12T15:54:58Z
```

# PERF401 extend suggestion has false negatives and makes code slower

---

_@hauntsaninja_

### Summary

Similar in spirit to https://github.com/astral-sh/ruff/issues/21890

Consider:
```python
import timeit


def main1():
    ret = []
    for x in range(8):
        for i in range(x):
            ret.append(i)


def main2():
    ret = []
    for x in range(8):
        ret.extend(range(x))


def main3():
    ret = []
    for x in range(8):
        for i in range(x):
            ret.append(str(i))  # PERF401 only complains about this


def main4():
    ret = []
    for x in range(8):
        ret.extend(str(i) for i in range(x))


if __name__ == "__main__":
    print(timeit.timeit(main1, number=10_000))
    print(timeit.timeit(main2, number=10_000))
    print(timeit.timeit(main3, number=10_000))
    print(timeit.timeit(main4, number=10_000))
```

```
λ python rf.py
0.007337332936003804
0.0059296670369803905
0.016938375076279044
0.036548584001138806
```

PERF401 wants to turn main3 into main4. This will make this code >2x slower.

PERF401 should want to turn main1 into main2, but doesn't.

### Version

_No response_

---

_Comment by @ntBre on 2025-12-10 16:21_

Thanks! Yeah there have been some other issues with the `extend` part of the rule too, such as https://github.com/astral-sh/ruff/issues/16315, especially. I wonder if we should consider deprecating that part of the rule.

I did manage to find at least one case where `main4` was faster, though, while playing with the `range` argument at different Python versions. For `range(100)` and Python 3.7, `main4` is a bit faster:

```py
import timeit

N = 100


def main3():
    ret = []
    for x in range(N):
        for i in range(x):
            ret.append(str(i))  # PERF401 only complains about this


def main4():
    ret = []
    for x in range(N):
        ret.extend(str(i) for i in range(x))


if __name__ == "__main__":
    print(timeit.timeit(main3, number=10_000))
    print(timeit.timeit(main4, number=10_000))
```

```shell
❯ uvx python@3.7 try.py
6.2967048669815995
5.577588776999619
```

Anyway, the first case does look like a false negative too.

---

_Label `rule` added by @ntBre on 2025-12-10 16:21_

---

_Label `bug` added by @AlexWaygood on 2025-12-11 10:49_

---

_Comment by @tdulcet on 2025-12-12 11:47_

It looks like the bigger issue here and with #21890 is the lack of support for nested for loops, as except when one can eliminate a generator altogether, a compensation is almost always faster and it removes the need for `extend`:
```py
import timeit

N = 100

def main1a():
	ret = []
	for x in range(N):
		for i in range(x):
			ret.append(i)

def main1b():
	ret = []
	for x in range(N):
		# ret.extend(i for i in range(x))
		ret.extend(range(x))  # <- Fastest without the generator

def main1c():
	ret = []
	ret.extend(i for x in range(N) for i in range(x))

def main1d():
	ret = [i for x in range(N) for i in range(x)]

def main2a():
	ret = []
	for x in range(N):
		for i in range(x):
			ret.append(str(i))

def main2b():
	ret = []
	for x in range(N):
		ret.extend(str(i) for i in range(x))
		# ret.extend(map(str, range(x)))  # <- Fastest without the generator

def main2c():
	ret = []
	ret.extend(str(i) for x in range(N) for i in range(x))

def main2d():
	ret = [str(i) for x in range(N) for i in range(x)]

if __name__ == "__main__":
	for func in (main1a, main1b, main1c, main1d, main2a, main2b, main2c, main2d):
		print(func.__name__, timeit.timeit(func, number=10000))
```
Python 3.12:
```
$ python3 try.py
main1a 1.0905432199999723
main1b 0.5453963160000512
main1c 2.4829541990000052
main1d 0.7715354909999519
main2a 4.433559850999984
main2b 4.579727133999995
main2c 4.705676019000009
main2d 3.890231156000027
```

---

_Comment by @Tishka17 on 2026-01-05 20:04_

```python
ITEMS = list(range(10000))

def append():
    a = []
    for i in ITEMS:
        if i%2:
            a.append(i)

def extend():
    a = []
    a.extend(i for i in ITEMS if i%2)


for foo in [append, extend]:
    print(foo.__name__, timeit.timeit(foo, number=10000))
```

My result:
```
append 1.3833269339520484
extend 1.521354956086725
```

Is it ever extend with generator expression faster that for-loop? 

---

_Comment by @hauntsaninja on 2026-01-05 21:11_

I think with current versions of the Python interpreter the for loop should basically always be faster.

---
