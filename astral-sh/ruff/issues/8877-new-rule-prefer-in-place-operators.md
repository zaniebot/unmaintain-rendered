---
number: 8877
title: "New rule: Prefer in-place operators"
type: issue
state: closed
author: Avasam
labels:
  - rule
  - help wanted
assignees: []
created_at: 2023-11-28T19:29:48Z
updated_at: 2024-04-12T03:08:51Z
url: https://github.com/astral-sh/ruff/issues/8877
synced_at: 2026-01-10T01:22:48Z
---

# New rule: Prefer in-place operators

---

_Issue opened by @Avasam on 2023-11-28 19:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I don't think any linter currently checks for this, although I did open a feature request in those I knew of where this feature might be in scope (https://github.com/MartinThoma/flake8-simplify/issues/188 and https://github.com/dosisod/refurb/issues/310). Whether they accept it, (and who knows when it would be implemented), this is still a check I'd like to see in Ruff as I don't directly use those other linters and autofixing should be possible. If you think this is a viable rule.

In-place operators lead to more concise code that is still readable. I'm not sure if there's any objective drawbacks (like a common pitfall for certain types). And performance-wise my understanding is that it should be faster (due to the in-place nature) or equivalent (thanks to Python duck-typing that will fallback to  `__add__` if `__iadd__` is not implemented).

Any of the following:
```python
some_string = (
  some_string
  + "a veeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeery long end of string"
)
index = index - 1
a_list = a_list + ["to concat"]
some_set = some_set | {"to concat"}
to_multiply = to_multiply * 5
to_divide = to_divide / 5
to_cube = to_cube ** 3
timeDiffSeconds = timeDiffSeconds % 60
flags = flags & 0x1
# etc.
```

Could be re-written as:
```python
some_string += "a veeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeery long end of string"
index -= 1
a_list += ["to concat"]
some_set |= {"to concat"}
to_multiply *= 5
to_divide /= 5
to_cube **= 3
timeDiffSeconds %= 60
flags &= 0x1
# etc.
```



---

_Comment by @zanieb on 2023-11-28 19:31_

Makes sense to me, I'm surprised this does not exist already.

---

_Label `rule` added by @zanieb on 2023-11-28 19:31_

---

_Comment by @Avasam on 2023-11-28 19:44_

I just thought of a special case with string interpolation that idk if should be included:
```python
# from
base_value = f"{base_value} end of string"

# to
base_value += " end of string"
```
Because it would break if `base_value` is not a `str` or a type that supports `+=` with a `str`.


---

_Comment by @zanieb on 2023-11-28 19:49_

We could support that conditional on the ability to determine that the type of `base_value` is `str`.

---

_Comment by @dhruvmanila on 2023-11-30 22:21_

We could first implement the simple cases of `var = var <operator> <expr>`. I think this could go into the `RUF` category. If anyone's interested, I'd be happy to give a look at the implementation.

---

_Label `help wanted` added by @dhruvmanila on 2023-11-30 22:21_

---

_Comment by @tdulcet on 2023-12-01 11:55_

Maybe this should be a separate rule, but there might be an opportunity for some more micro optimizations here. For example, if it were faster, these:
```py
a_list += ["to concat"]
a_list += ["to concat 1", "to concat 2"]
some_set |= {"to concat"}
some_set |= {"to concat 1", "to concat 2"}
# ect.
```
could potenchally be further rewritten as:
```py
a_list.append("to concat")
a_list.extend(("to concat 1", "to concat 2"))
some_set.add("to concat")
some_set.update(("to concat 1", "to concat 2"))
# ect.
```

---

_Comment by @Avasam on 2023-12-01 17:46_

Increased complexity, may require type information, and I'm not asking for this for a first version / as part of my original feature request, but worth considering: https://github.com/dosisod/refurb/issues/310#issuecomment-1835466992

> Because anyone can make their own custom in-place operators, I think it is best to only support built-in types, though there [could] be an option to extend this to all types [...]
> 
> Also, I'd probably add these to your list as well:
> 
> ```python
> a = a or b
> c = c and d
> ```
> 
> Re-write as:
> 
> ```python
> a |= b
> c &= d
> ```
> 
> assuming `a`, `b`, `c` and `d` are `bool` types.

---

> Maybe this should be a separate rule, but there might be an opportunity for some more micro optimizations here. For example, if it were faster, these:

Different rule imo, and may require type information. But worth investigating. Even if the performance is negligible or too variant between Python versions, it could become a readability/preference rule.


---

_Referenced in [astral-sh/ruff#9932](../../astral-sh/ruff/pulls/9932.md) on 2024-02-11 16:27_

---

_Comment by @lshi18 on 2024-02-11 19:34_

Hi, I've made an attempt implementing this rule. Please help to have a review and let me know your thoughts. Thanks.

---

_Comment by @charliermarsh on 2024-04-12 03:08_

Closed by https://github.com/astral-sh/ruff/pull/9932.

---

_Closed by @charliermarsh on 2024-04-12 03:08_

---

_Referenced in [astral-sh/ruff#13533](../../astral-sh/ruff/issues/13533.md) on 2024-10-07 09:59_

---

_Referenced in [astral-sh/ruff#14636](../../astral-sh/ruff/issues/14636.md) on 2024-11-27 16:01_

---
