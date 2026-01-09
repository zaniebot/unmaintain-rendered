---
number: 7728
title: Use str.casefold() instead of str.lower() for comparison.
type: issue
state: open
author: Jeremiah-England
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-10-01T00:05:17Z
updated_at: 2023-10-02T17:49:51Z
url: https://github.com/astral-sh/ruff/issues/7728
synced_at: 2026-01-07T13:12:15-06:00
---

# Use str.casefold() instead of str.lower() for comparison.

---

_Issue opened by @Jeremiah-England on 2023-10-01 00:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I'm reading _Fluent Python_ right now and the author introduced me to using `casefold()` instead of `lower()` for string comparison. This seems to be the general recommendation. See https://stackoverflow.com/questions/45745661/lower-vs-casefold-in-string-matching-and-converting-to-lowercase. 

It seems like we could have a rule like this:

```python
s1.lower() == s2.lower()  # Error: Use str.casefold() for caseless string comparison
s1.lower() != s2.lower()  # Error: Use str.casefold() for caseless string comparison
```

I tested the performance and it looks like they are about the same.

<details><summary>Performance testing</summary>

```python
import random
import timeit

long = bytes(random.randint(0, 255) for _ in range(1_000_000)).decode("utf-8", errors="ignore")
short = bytes(random.randint(0, 255) for _ in range(1_000)).decode("utf-8", errors="ignore")
tiny = bytes(random.randint(0, 255) for _ in range(10)).decode("utf-8", errors="ignore")

print(f"Length of long: {len(long)}")
print(f"Length of short: {len(short)}")
print(f"Length of tiny: {len(tiny)}")


long_lower_time = timeit.timeit("long.lower()", globals=globals(), number=100)
long_casefold_time = timeit.timeit("long.casefold()", globals=globals(), number=100)
print(f"{long_lower_time=}")
print(f"{long_casefold_time=}")

short_lower_time = timeit.timeit("short.lower()", globals=globals(), number=100_000)
short_casefold_time = timeit.timeit("short.casefold()", globals=globals(), number=100_000)
print(f"{short_lower_time=}")
print(f"{short_casefold_time=}")

tiny_lower_time = timeit.timeit("tiny.lower()", globals=globals(), number=10_000_000)
tiny_casefold_time = timeit.timeit("tiny.casefold()", globals=globals(), number=10_000_000)
print(f"{tiny_lower_time=}")
print(f"{tiny_casefold_time=}")

# Length of long: 533764
# Length of short: 543
# Length of tiny: 2
# long_lower_time=0.7035579030052759
# long_casefold_time=0.7010332670179196
# short_lower_time=0.6138628749758936
# short_casefold_time=0.5890167159959674
# tiny_lower_time=0.5530402820440941
# tiny_casefold_time=0.5944054980063811
```

</details>

The main risk seems to be other classes with a `.lower()` method that someone might use for comparison. It seems that should be unlikely enough that a rule would be OK. I searched private codebase and the results for `lower.*[!=]=.*lower` were all string comparisons.

---

_Label `rule` added by @charliermarsh on 2023-10-01 03:30_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-01 03:30_

---

_Comment by @konstin on 2023-10-02 17:11_

To me, `casefold` and `lower` have different semantics and you shouldn't just replace one with another. For example, in python packaging, the casing of package names generally doesn't matter, e.g. `asgiref`, `Asgiref` and `AsgiRef` will both give you the desired package, since `"asgiref".lower() == "Asgiref".lower() == "AsgiRef".lower()`. `aßgiref` otoh is an invalid package name and should not be accepted. This is also represented by pypi:
Works: https://pypi.org/project/asgiref/
Works: https://pypi.org/project/Asgiref/, redirects to https://pypi.org/project/asgiref/
Broken: https://pypi.org/project/aßgiref/

(Personally, i find `"ß".upper() == "SS"` a strange semantic already and one that does not match my feeling for language, plus the captial ß now exists (https://en.wikipedia.org/wiki/%C3%9F#Development_of_a_capital_form), but i can see how that's something you want to support now that it's a fixed part of unicode, especially when dealing which natural language rather than identifiers)

---

_Comment by @zanieb on 2023-10-02 17:49_

I've never seen anyone actually use this but it does seem like a reasonable suggestion. 

---
