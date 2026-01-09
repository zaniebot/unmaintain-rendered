---
number: 2400
title: Q000 for multiline strings with different quotes
type: issue
state: closed
author: samuelcolvin
labels:
  - bug
assignees: []
created_at: 2023-01-31T16:29:59Z
updated_at: 2023-02-01T00:24:46Z
url: https://github.com/astral-sh/ruff/issues/2400
synced_at: 2026-01-07T13:12:14-06:00
---

# Q000 for multiline strings with different quotes

---

_Issue opened by @samuelcolvin on 2023-01-31 16:29_

code:

```py
    assert s.to_python(123) == (
        "123 info=SerializationInfo(include=None, exclude=None, mode='python', by_alias=True, exclude_unset=False, "
        "exclude_defaults=False, exclude_none=False, round_trip=False)"
    )
```

Is resulting in "Q000 Double quotes found but single quotes preferred"

As you can see the first line has a single quote in it, so needs to use double quotes, the second line doesn't but by conversion you'd expect to keep using double quotes for the whole multi-line string.


---

_Comment by @samuelcolvin on 2023-01-31 16:32_

Tried on latest ruff, getting the same error.

---

_Label `bug` added by @charliermarsh on 2023-01-31 17:25_

---

_Comment by @charliermarsh on 2023-01-31 17:25_

Makes sense! Thanks for filing.

---

_Comment by @samuelcolvin on 2023-01-31 17:27_

Just for reference, same error reported on flake8-quotes by me in 2019 - https://github.com/zheller/flake8-quotes/issues/82

---

_Comment by @charliermarsh on 2023-01-31 17:36_

Hopefully I can fix it today :)

---

_Comment by @charliermarsh on 2023-01-31 19:17_

Do you think this should be an _error_, or merely permitted?

```py
    assert s.to_python(123) == (
        "123 info=SerializationInfo(include=None, exclude=None, mode='python', by_alias=True, exclude_unset=False, "
        'exclude_defaults=False, exclude_none=False, round_trip=False)'
    )
```

---

_Comment by @samuelcolvin on 2023-01-31 19:41_

personally I would say an error, but I'm fine with permitted too, especially since you had to do that (or add `noqa`) until now.

---

_Comment by @charliermarsh on 2023-01-31 20:02_

I did some testing -- I probably need to treat it as "permitted", because Black changes them back. That is, Black changes this:

```py
(
    '"abc"'
    'abc'
)
```

...to...

```py
(
    '"abc"'
    "abc"
)
```

(Black will also truncate the whitespace, but omitted that change for clarity.)

It wouldn't be a huge deal in practice, since if you're using this plugin with `"double"` preference, you're compatible with Black anyway; and if you're using it with `"single"` preference, you're already incompatible with Black. But I'd prefer to avoid the potential of introducing a fix loop.


---

_Comment by @samuelcolvin on 2023-01-31 20:08_

agreed

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-31 20:11_

---

_Referenced in [astral-sh/ruff#2416](../../astral-sh/ruff/pulls/2416.md) on 2023-01-31 21:10_

---

_Closed by @charliermarsh on 2023-01-31 21:27_

---

_Comment by @samuelcolvin on 2023-01-31 21:42_

Wonderful, thanks.

---

_Comment by @charliermarsh on 2023-01-31 21:47_

Is there any branch etc. that I could test this on just to ensure compatibility? :)

It'll go out in tonight's release.

---

_Comment by @samuelcolvin on 2023-01-31 21:49_

See https://github.com/pydantic/pydantic-core/pull/374, there's currently a bunch of `noqa`s.

---

_Comment by @charliermarsh on 2023-01-31 21:59_

👍 Thanks, looking good on that branch

---

_Comment by @samuelcolvin on 2023-02-01 00:24_

👍 (on mobile, so this is a reaction)

---

_Referenced in [astral-sh/ruff#7889](../../astral-sh/ruff/issues/7889.md) on 2023-10-12 04:57_

---

_Referenced in [astral-sh/ruff#8265](../../astral-sh/ruff/issues/8265.md) on 2023-10-26 22:45_

---

_Referenced in [astral-sh/ruff#11209](../../astral-sh/ruff/issues/11209.md) on 2024-04-30 03:46_

---
