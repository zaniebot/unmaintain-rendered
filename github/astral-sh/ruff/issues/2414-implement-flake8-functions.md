---
number: 2414
title: "Implement `flake8-functions`"
type: issue
state: open
author: ngnpope
labels:
  - plugin
assignees: []
created_at: 2023-01-31T20:59:46Z
updated_at: 2024-04-09T10:24:04Z
url: https://github.com/astral-sh/ruff/issues/2414
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement `flake8-functions`

---

_Issue opened by @ngnpope on 2023-01-31 20:59_

[GitHub](https://github.com/best-doctor/flake8-functions), [PyPI](https://pypi.org/project/flake8-functions/).

- [ ] [`CFQ001`](https://github.com/best-doctor/flake8-functions/blob/master/flake8_functions/function_length.py): Function ... has length {function_length} that exceeds max allowed length {max_function_length}
  - This is an alternative to complexity checkers and is just purely the number of lines in the function
  - Unlike `too-many-statements` (`PLR0915`) from `pylint`, this counts the literal number of lines that the function takes up on the page, rather than the number of statements in the function.
- [x] [`CFQ002`](https://github.com/best-doctor/flake8-functions/blob/master/flake8_functions/function_arguments_amount.py): Function ... has {arguments_amount} arguments that exceeds max allowed {max_parameters_amount}
  - Already implemented as `too-many-arguments` (`PLR0913`) from [`pylint`](https://github.com/charliermarsh/ruff/issues/970/)
- [ ] [`CFQ003`](https://github.com/best-doctor/flake8-functions/blob/master/flake8_functions/function_purity.py): Function ... is not pure
  - Uses [`mr-proper`](https://pypi.org/project/mr-proper/) under the hood to check that a function is "pure"
  - _That library says it's very experimental and it looks a bit too magic, so can probably skip this rule._
- [x] [`CFQ004`](https://github.com/best-doctor/flake8-functions/blob/master/flake8_functions/function_returns_amount.py): Function ... has {returns_amount} returns that exceeds max allowed {max_returns_amount}
  - Already implemented as `too-many-return-statements` (`PLR0911`) from [`pylint`](https://github.com/charliermarsh/ruff/issues/970/)

Configuration options:
- [ ] `max-function-length` implemented as `pylint:max-statements`
- [x] `max-parameters-amount` implemented as `pylint:max-args`
- [x] `max-returns-amount` to be implemented as `pylint:max-returns`

Other than `CFQ003` which is dubious, these will all be covered by `pylint` rules...
So this can be closed if desired, but this will be useful to refer to when aliases are implemented.

---

_Comment by @sbrugman on 2023-01-31 22:44_

CFQ003 would be truly awesome to have 

---

_Comment by @ngnpope on 2023-01-31 23:24_

Sounds like "challenge accepted"? 😂

---

_Comment by @sbrugman on 2023-01-31 23:34_

The features of Mr. Proper should be useable attributes to discover violations:

> - that function has no blacklisted calls (like print) and blacklisted attributes access (like smth.count);
> - that function not uses global objects (only local vars and function arguments);
> - that function has al least one return;
> - that function not mutates it's arguments;
> - that function has no local imports;
> - that function has no arguments of forbidden types (like ORM objects);
> - that function not uses self, class or super;
> - that function has calls of only pure functions.

Mostly this requires no type checking!

---

_Label `plugin` added by @charliermarsh on 2023-02-01 15:09_

---

_Comment by @chanman3388 on 2023-02-03 19:14_

You mind if I implement `too-many-return-statements`?

---

_Comment by @ngnpope on 2023-02-03 19:42_

Go ahead! 🙂

---

_Referenced in [astral-sh/ruff#2564](../../astral-sh/ruff/pulls/2564.md) on 2023-02-04 11:31_

---

_Comment by @mirecl on 2023-05-15 07:13_

Why [CFQ001](https://github.com/best-doctor/flake8-functions/blob/master/flake8_functions/function_length.py) and [CFQ004](https://github.com/best-doctor/flake8-functions/blob/master/flake8_functions/function_returns_amount.py) is done?
PLR0915/PLR0911 is not equal rule for CFQ001/CFQ004!

---

_Comment by @AlexWaygood on 2024-04-09 10:24_

> Why [CFQ001](https://github.com/best-doctor/flake8-functions/blob/master/flake8_functions/function_length.py?rgh-link-date=2023-05-15T07%3A13%3A05Z) and [CFQ004](https://github.com/best-doctor/flake8-functions/blob/master/flake8_functions/function_returns_amount.py?rgh-link-date=2023-05-15T07%3A13%3A05Z) is done? PLR0915/PLR0911 is not equal rule for CFQ001/CFQ004!

I've unticked `CFQ001` -- you're right, that does seem distinct from `PLR0915` to me. `CFQ004` _does_ look basically the same as `PLR0911` to me, though -- would you mind explaining to me how they are different, @mirecl?

(N.B. Just because I've "unticked" the rule doesn't _necessarily_ mean that we'll accept a PR. `CFQ001` is a very opinionated rule, so we probably _wouldn't_ accept this rule until https://github.com/astral-sh/ruff/issues/1774 is completed and we have a better way of marking rules as disabled by default.)

---
