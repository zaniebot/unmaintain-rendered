```yaml
number: 3482
title: Implement flake8-dunder-all
type: issue
state: open
author: Goldziher
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-03-13T15:39:45Z
updated_at: 2023-07-10T01:26:10Z
url: https://github.com/astral-sh/ruff/issues/3482
synced_at: 2026-01-12T15:54:43Z
```

# Implement flake8-dunder-all

---

_@Goldziher_

Would be great to see, also the optional script there that auto adds it: 

https://github.com/python-formate/flake8-dunder-all

One note- `__all__` should be a tuple is a good optional rule, since it's a convention I'm some code bases and gives a very small performance gain..

---

_Label `plugin` added by @charliermarsh on 2023-03-13 16:39_

---

_Comment by @onerandomusername on 2023-03-14 05:03_

Interesting, that library seems to be missing two key checks.

- https://pypi.org/project/sort-all/ to sort the dunders in order
- the fact that every item in an __all__ should be a string (and ruff could probably fix that automatically)

---

_Comment by @Goldziher on 2023-03-14 15:41_

> Interesting, that library seems to be missing two key checks.
> 
> - https://pypi.org/project/sort-all/ to sort the dunders in order
> - the fact that every item in an __all__ should be a string (and ruff could probably fix that automatically)

Also:

3. ensure uniqueness of the strings
4. ensure the string is identical to a name in the module namespace (i.e. is help by an actual value)

---

_Comment by @edgarrmondragon on 2023-03-30 20:44_

The original flake8 plugin seems to be unmaintained. What do folks think of porting and adding the rules suggested above (prefix is a placeholder):

- `DALL000`: Module lacks `__all__`
- `DALL001`: `__all__` is out of order (auto-fixable)
- ~`DALL002`: Element of `__all__` is not a string literal~. Covered by Pylint `invalid-all-object`
- `DALL003`: Elements of `__all__` should be unique (auto-fixable)
- ~`DALL004`: ... specified in `__all__` but not present in module~. Covered by Pyflakes `undefined-export`

---

_Comment by @onerandomusername on 2023-03-31 00:27_

it would also be nice to check the following
- `__all__` should be a tuple
- `__all__` should be a list

all can be either of the above, as some `__all__`s need to be extended later on.

---

_Comment by @charliermarsh on 2023-03-31 00:33_

Some of these exist as already-implemented Pylint rules (`invalid-all-format` and `invalid-all-object`).

---

_Comment by @edgarrmondragon on 2023-03-31 01:12_

Thanks @charliermarsh, I've updated my comment to reflect which checks are already implemented

---

_Comment by @onerandomusername on 2023-03-31 02:42_

> Some of these exist as already-implemented Pylint rules (`invalid-all-format` and `invalid-all-object`).

Tbh they could be autofixable. The information from F822 is there for PLE0604 to be autofixable.

---

_Comment by @JackAshwell11 on 2023-04-11 17:05_

Has anymore progress been made on this issue? Having this in Ruff would be pretty helpful.

I can implement the rules if it is to be accepted into Ruff

---

_Comment by @edgarrmondragon on 2023-04-25 02:26_

@Aspect1103 are you still interested on working on this issue? Otherwise I might take a look in the coming days.

---

_Comment by @JackAshwell11 on 2023-04-25 07:06_

> @Aspect1103 are you still interested on working on this issue? Otherwise I might take a look in the coming days.

I have gotten quite busy in the past couple days so haven't had much time. You're free to work on it if you want

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---
