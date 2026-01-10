```yaml
number: 351
title: "Automatically rewrite `NamedTuple` and `TypedDict` with newer syntaxes"
type: issue
state: closed
author: charliermarsh
labels:
  - rule
assignees: []
created_at: 2022-10-07T20:07:48Z
updated_at: 2022-11-20T15:26:33Z
url: https://github.com/astral-sh/ruff/issues/351
synced_at: 2026-01-10T12:09:58Z
```

# Automatically rewrite `NamedTuple` and `TypedDict` with newer syntaxes

---

_Issue opened by @charliermarsh on 2022-10-07 20:07_

From `pyupgrade`:

<img width="792" alt="Screen Shot 2022-10-07 at 4 04 06 PM" src="https://user-images.githubusercontent.com/1309177/194644709-69165fc1-6da3-4551-abcc-4d86e1801036.png">

This could be a good use-case for the codegen module.


---

_Label `rule` added by @charliermarsh on 2022-10-07 20:07_

---

_Comment by @martinlehoux on 2022-11-12 18:20_

I'll try this one if that's okay

---

_Comment by @martinlehoux on 2022-11-13 09:11_

A few questions on top of my mind @charliermarsh 

- Should we handle `from typing import TypedDict as CustomTypedDict`?
- Should we handle `MyType = TypedDict("MyType", dict(a=str, b=int))`?
- What should we do for `MyType = TypedDict("OtherName", ...)`?
- And overall, do you think TypedDict rule and NamedTuple rule should be the same, or two different rules?

---

_Comment by @charliermarsh on 2022-11-13 15:51_

Good questions!

> Should we handle `from typing import TypedDict as CustomTypedDict`?

Na, I think we should ignore (leave it unchanged).

> Should we handle `MyType = TypedDict("MyType", dict(a=str, b=int))`?

I think it's fine not to handle this case. It'd be nice to include it, but I don't see it as a requirement.

> What should we do for `MyType = TypedDict("OtherName", ...)`?

Let's ignore this case, if possible (leave it unchanged).

> And overall, do you think TypedDict rule and NamedTuple rule should be the same, or two different rules?

I would vote for two different rules.


---

_Comment by @martinlehoux on 2022-11-18 18:54_

Regarding [NamedTuples](https://docs.python.org/3/library/collections.html#collections.namedtuple):
- Support `rename` (that converts invalid identifiers to  `_<k>` keys, and duplicated too) => _I  don't see a technical reason for why not, but doesn't seem very useful_
- Support `defaults` (that list fields defaults from right to left) => _I guess this one is useful_
- Support `module` (I don't event know if possible with class) => _I'd say we drop that_

---

_Closed by @charliermarsh on 2022-11-20 15:26_

---

_Comment by @charliermarsh on 2022-11-20 15:26_

Completed by @martinlehoux! Thank you!

---
