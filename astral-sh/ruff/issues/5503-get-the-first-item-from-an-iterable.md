```yaml
number: 5503
title: Get the first item from an iterable
type: issue
state: closed
author: hoxbro
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-07-04T15:19:55Z
updated_at: 2023-07-10T16:32:49Z
url: https://github.com/astral-sh/ruff/issues/5503
synced_at: 2026-01-10T11:09:47Z
```

# Get the first item from an iterable

---

_Issue opened by @hoxbro on 2023-07-04 15:19_

Sometimes I need to get the first item of an iterable like a `dict_keys()` object. A natural way to do this is to do it with `list(...)[0]`, though it can be much faster to do it with `next(iter(...))`. 

I have tried to see if a rule already describes this, but I could not find it.

Some examples: 

``` yaml
Examples with list(...)[0] with length of 1000
Dictionary:	1000 loops, best of 5: 4.35 usec per loop
List:		1000 loops, best of 5: 1 usec per loop
Range:		1000 loops, best of 5: 5.6 usec per loop

Examples with next(iter(...)) with length of 1000
Dictionary:	1000 loops, best of 5: 34.3 nsec per loop
List:		1000 loops, best of 5: 33.1 nsec per loop
Range:		1000 loops, best of 5: 27.7 nsec per loop

```

<details> 

<summary> Code </summary>


``` bash


N=1000
echo "Examples with list(...)[0] with length of $N"
echo -n -e "Dictionary:\t"
python -m timeit --s="a = {i: i for i in range($N)}" --n 1000 "list(a)[0]"
echo -n -e "List:\t\t"
python -m timeit --s="a = [i for i in range($N)]" --n 1000 "list(a)[0]"
echo -n -e "Range:\t\t"
python -m timeit --s="a = range($N)" --n 1000 "list(a)[0]"

echo -e "\nExamples with next(iter(...)) with length of $N"
echo -n -e "Dictionary:\t"
python -m timeit --s="a = {i: i for i in range($N)}" --n 1000 "next(iter(a))"
echo -n -e "List:\t\t"
python -m timeit --s="a = [i for i in range($N)]" --n 1000 "next(iter(a))"
echo -n -e "Range:\t\t"
python -m timeit --s="a = range($N)" --n 1000 "next(iter(a))"

```
</details>




---

_Comment by @dhruvmanila on 2023-07-04 15:40_

This is not only faster but would consume less memory as it doesn't need to create the entire list to access the only element :)

---

_Label `rule` added by @dhruvmanila on 2023-07-04 15:40_

---

_Comment by @evanrittenhouse on 2023-07-05 00:52_

You can assign this to me. I'll add it as a `RUF` rule.

---

_Assigned to @evanrittenhouse by @charliermarsh on 2023-07-05 01:58_

---

_Comment by @charliermarsh on 2023-07-05 01:58_

This looks reasonable, but let's be really careful in reviewing any violations that pop up in the ecosystem CI.

---

_Comment by @evanrittenhouse on 2023-07-05 02:34_

@charliermarsh Yup!

---

_Label `accepted` added by @charliermarsh on 2023-07-10 01:28_

---

_Closed by @charliermarsh on 2023-07-10 16:32_

---
