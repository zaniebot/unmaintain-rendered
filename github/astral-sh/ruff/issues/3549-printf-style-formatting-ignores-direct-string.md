---
number: 3549
title: "`printf-style-formatting` ignores direct-string interpolation"
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - fixes
assignees: []
created_at: 2023-03-15T19:59:28Z
updated_at: 2023-03-21T03:48:07Z
url: https://github.com/astral-sh/ruff/issues/3549
synced_at: 2026-01-07T13:12:14-06:00
---

# `printf-style-formatting` ignores direct-string interpolation

---

_Issue opened by @charliermarsh on 2023-03-15 19:59_

Given:

```py
"%s" % "foo"
"%s" % ("foo",)
```

Ruff will raise UP031 for the latter, but not the former.


---

_Label `autofix` added by @charliermarsh on 2023-03-15 19:59_

---

_Label `good first issue` added by @charliermarsh on 2023-03-16 00:08_

---

_Referenced in [astral-sh/ruff#3554](../../astral-sh/ruff/issues/3554.md) on 2023-03-16 07:00_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-18 02:21_

---

_Referenced in [astral-sh/ruff#3600](../../astral-sh/ruff/pulls/3600.md) on 2023-03-18 19:25_

---

_Closed by @charliermarsh on 2023-03-21 03:35_

---

_Comment by @globau on 2023-03-21 03:43_

🎉 

---

_Comment by @charliermarsh on 2023-03-21 03:48_

Sadly the most common case isn't safely fixable right now. I want to improve it... Sort of a long story, but if you have:

```py
print("%s" % x)
```

It turns out we _can't_ safely convert that to:

```py
print("{}".format(x))
```

Because `x` _could_ be a single-element tuple, like so:

```py
x = (1,)
print("%s" % x)
print("{}".format(x))
```

So for now, we can only convert these for cases in which the placeholder has more than one positional argument, or at least one keyword argument (in which case, we _know_ it's a tuple or a mapping respectively).

Though we do now fix cases like:

```py
"%d" % 1
"%s" % "foo"
"%s" % f"foo{bar}"
```

---

_Referenced in [astral-sh/ruff#6796](../../astral-sh/ruff/issues/6796.md) on 2023-08-22 23:24_

---

_Referenced in [astral-sh/ruff#7579](../../astral-sh/ruff/issues/7579.md) on 2023-09-21 19:25_

---
