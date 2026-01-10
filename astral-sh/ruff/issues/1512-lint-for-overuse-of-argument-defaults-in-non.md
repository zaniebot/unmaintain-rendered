```yaml
number: 1512
title: lint for overuse of argument defaults in non-public functions
type: issue
state: open
author: danieleades
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2022-12-31T15:17:32Z
updated_at: 2023-07-10T01:33:26Z
url: https://github.com/astral-sh/ruff/issues/1512
synced_at: 2026-01-10T11:09:43Z
```

# lint for overuse of argument defaults in non-public functions

---

_Issue opened by @danieleades on 2022-12-31 15:17_

i'm working in a large, public python codebase that lints using ruff.

I have a general *feeling* that this codebase has massive overuse of argument defaults in methods and I would love to lint for this. This happens in a couple of different ways-

- a non-public function has many arguments with default values, however it is never called without fully-specifying the default values anywhere
- functions that repeatedly delegate to lower-level functions, but repeat the default arguments redundantly at every level in the hierarchy

it's the second case here that causes the biggest headaches in this particular codebase. The function signatures are very noisy. Also (in my case) the default value is almost always `None`, and it takes some serious archaeology to try and figure out what it means to leave out any particular arg (`None` is often semantically different to `not None`).

I have no idea whether this type of lint is possible using this library. You'd have to collect every non-public function with default valued args, and compare them to each of their call-sites to determine whether the arg defaults are ever used or not. Or something.


---

_Label `rule` added by @charliermarsh on 2022-12-31 18:11_

---

_Comment by @danieleades on 2023-01-01 16:01_

crossposted at https://github.com/MartinThoma/flake8-simplify/issues/165

---

_Comment by @danieleades on 2023-01-01 16:55_

I'd be happy to take a stab at this if someone could point me in the right direction?

---

_Comment by @charliermarsh on 2023-01-01 17:56_

I think this is out-of-scope _right now_ because we don't do cross-file checks. That is, if you define a function in one file, we don't have a way to pick up its signature when linting files that import it.

I want to get to that in the future, but the model right now is single-file oriented.

---

_Comment by @danieleades on 2023-01-01 19:14_

> I think this is out-of-scope _right now_ because we don't do cross-file checks. That is, if you define a function in one file, we don't have a way to pick up its signature when linting files that import it.
> 
> I want to get to that in the future, but the model right now is single-file oriented.

I wondered about that. Fair enough 

---

_Comment by @danieleades on 2023-01-03 08:16_

> I think this is out-of-scope _right now_ because we don't do cross-file checks. That is, if you define a function in one file, we don't have a way to pick up its signature when linting files that import it.
> 
> I want to get to that in the future, but the model right now is single-file oriented.

you could still implement this in a limited capacity today. This could still apply to any function which is private to a single file, has default arguments, and those defaults are never used.

function visibility would be

- private if a leading underscore (and no `__all__`)
- public if no leading underscore (and no `__all__`)
- private if not in `__all__` (if present)

you'd miss most cases of course, but you'd have the logic in place

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:33_

---
