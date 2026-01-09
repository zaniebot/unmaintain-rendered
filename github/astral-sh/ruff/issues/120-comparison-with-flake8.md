---
number: 120
title: Comparison with flake8?
type: issue
state: closed
author: samuelcolvin
labels:
  - documentation
assignees: []
created_at: 2022-09-07T12:37:14Z
updated_at: 2022-09-07T14:36:48Z
url: https://github.com/astral-sh/ruff/issues/120
synced_at: 2026-01-07T13:12:14-06:00
---

# Comparison with flake8?

---

_Issue opened by @samuelcolvin on 2022-09-07 12:37_

As mentioned in #119, ruff looks great and I'd love to adopt it in pydantic, however I'd like to have a better understanding of what I loose by switching from flake8 to ruff?

I've run ruff on pydantic and apart from #119, ruff has found a few legitimate things that I would either fix or ignore. The problem is what checks am I no longer getting?

This is harder to find without either a lot of trial or waiting to get bitten.

I would therefore find it really helpful if there was a good comparison of ruff and flake8. I'm sure other potential users would also find this helpful.

---

_Comment by @charliermarsh on 2022-09-07 13:20_

Great suggestion -- I have an answer in my head but it'd be good to both formalize it and get it down on "paper". Will write something here + add to the README.


---

_Label `documentation` added by @charliermarsh on 2022-09-07 14:15_

---

_Comment by @charliermarsh on 2022-09-07 14:32_

Added this note to the README (feedback welcome):

```
### Parity with Flake8

ruff's goal is to achieve feature-parity with Flake8 when used (1) without any plugins,
(2) alongside Black, and (3) on Python 3 code. (Using Black obviates the need for many of Flake8's
stylistic checks; limiting to Python 3 obviates the need for certain compatibility checks.)

Under those conditions, Flake8 implements about 58 rules, give or take. At time of writing, ruff
implements 24 rules. (Note that these 24 rules likely cover a disproportionate share of errors:
unused imports, undefined variables, etc.)

Of the unimplemented rules, ruff is missing:

- 15 rules related to string `.format` calls.
- 6 rules related to misplaced `yield`, `break`, and `return` statements.
- 3 rules related to syntax errors in doctests and annotations.
- 2 rules related to redefining unused names.

...along with a variety of others that don't fit neatly into any specific category.

Beyond rule-set parity, ruff suffers from the following limitations vis-à-vis Flake8:

1. Flake8 supports a wider range of `noqa` patterns, such as per-file ignores defined in `.flake8`.
2. Flake8 has a plugin architecture and supports writing custom lint rules.
3. ruff does not yet support parenthesized context managers.```

---

_Closed by @charliermarsh on 2022-09-07 14:32_

---

_Comment by @samuelcolvin on 2022-09-07 14:36_

Thanks so much, that's really useful.

Once #119 is fixed, I think I'll use both ruff and flake8, but use ruff in pre-commit where performance is most critical. That will also allow me to see differences between the two in the real world.

---

_Referenced in [astral-sh/ruff#161](../../astral-sh/ruff/issues/161.md) on 2022-09-12 00:38_

---
