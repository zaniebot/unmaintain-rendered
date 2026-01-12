```yaml
number: 3965
title: "feat: ruff rule to replace `.pop(...)` with `del dict[...]` when the return is unused"
type: issue
state: closed
author: markis
labels:
  - rule
assignees: []
created_at: 2023-04-13T20:34:42Z
updated_at: 2023-04-14T02:31:37Z
url: https://github.com/astral-sh/ruff/issues/3965
synced_at: 2026-01-12T15:54:44Z
```

# feat: ruff rule to replace `.pop(...)` with `del dict[...]` when the return is unused

---

_@markis_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

When using a dictionary, a developer could use the `dict.pop(key)` or `del dict[key]` syntax to remove an item from a dictionary.  The `pop` syntax has the additional feature of returning the value from the dictionary, but it comes with a slight performance cost over the `del` syntax.  It would be nice if ruff could identify places that the return from the `pop` call was unused and would suggest the `del` instead.

Example
```python
fruits = { 'apples': 3, 'pears': 4, 'bananas': 1 }

fruits.pop('apples') # <-- suggest that developer change this to `del fruits['apples']`
del fruits['pears']

print(fruits)  # output: {'bananas':1}
```


---

_Label `rule` added by @charliermarsh on 2023-04-13 20:35_

---

_Comment by @dhruvmanila on 2023-04-14 01:52_

Do you have any benchmark to support the performance difference? I'm doing a preliminary analysis and it shows no significant difference:

```python
In [21]: %timeit d = {chr(i): i for i in range(10000)}; del d[chr(456)]
1.29 ms ± 55.5 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)

In [22]: %timeit d = {chr(i): i for i in range(10000)}; d.pop(chr(456))
1.25 ms ± 14.6 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
```

---

_Comment by @dhruvmanila on 2023-04-14 01:56_

Another thing to note is this example:

```python
fruits = {'apples': 3, 'pears': 4, 'bananas': 1}

# This will raise a KeyError
fruits.pop('oranges')

# This will _not_ raise a KeyError, so we can't recommend to use `del fruits['oranges']`
fruits.pop('oranges', None)
```

---

_Comment by @markis on 2023-04-14 02:21_

> Do you have any benchmark to support the performance difference? I'm doing a preliminary analysis and it shows no significant difference:
> 
> ```python
> In [21]: %timeit d = {chr(i): i for i in range(10000)}; del d[chr(456)]
> 1.29 ms ± 55.5 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
> 
> In [22]: %timeit d = {chr(i): i for i in range(10000)}; d.pop(chr(456))
> 1.25 ms ± 14.6 µs per loop (mean ± std. dev. of 7 runs, 1,000 loops each)
> ```

Earlier, I ran a similar test on my machine (python 3.10 on an M1) and `del` was definitely more performant and a quick google search confirmed [my bias](https://stackoverflow.com/questions/60425973/which-is-faster-list-pop0-vs-del-list0).

But after you mentioned that, I ran the same test on a production python server.  And seems like the timings are about the same.  Closing as it's environment specific and not globally true.  Thanks for checking, @dhruvmanila !

---

_Closed by @markis on 2023-04-14 02:21_

---

_Comment by @dhruvmanila on 2023-04-14 02:31_

> Thanks for checking, @dhruvmanila !

Happy to help! :)

---
