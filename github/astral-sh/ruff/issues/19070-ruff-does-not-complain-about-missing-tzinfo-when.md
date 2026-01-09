---
number: 19070
title: "Ruff does not complain about missing tzinfo when using `datetime.datetime.combine`"
type: issue
state: open
author: RuurdBijlsma
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-01T13:57:02Z
updated_at: 2025-07-01T14:10:34Z
url: https://github.com/astral-sh/ruff/issues/19070
synced_at: 2026-01-07T13:12:16-06:00
---

# Ruff does not complain about missing tzinfo when using `datetime.datetime.combine`

---

_Issue opened by @RuurdBijlsma on 2025-07-01 13:57_

### Summary

Example when leaving out tzinfo when constructing a datetime normally:

```python
1 file reformatted, 87 files left unchanged
src/dynamic_dimensioning_logic/logic.py:78:13: DTZ001 `datetime.datetime()` called without a `tzinfo` argument
   |
76 |         self.hello_world.write(output_df=output_df)
77 |         logger.info("posted-hello_world")
78 |         x = datetime.datetime(year=2025, month=6, day=1, hour=0, minute=0, second=0)
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ DTZ001
79 |         logger.info("x", x=x)
   |
   = help: Pass a `datetime.timezone` object to the `tzinfo` parameter

Found 1 error.
```

Now, when I construct a datetime with datetime.datetime.combine (note I don't pass a tzinfo):

```python
y = datetime.datetime.combine(
    datetime.date(year=2025, month=6, day=1),
    datetime.time(hour=0, minute=0, second=0)
)
```

I get no ruff error, even though combine does take a tzinfo argument. This cause a bug for me so I'd love a ruff error here.

I have `select =['ALL']` in my ruff.toml, so it's not disabled.


### Version

0.12.1

---

_Label `rule` added by @ntBre on 2025-07-01 14:08_

---

_Label `needs-decision` added by @ntBre on 2025-07-01 14:08_

---

_Comment by @ntBre on 2025-07-01 14:10_

Thanks, this sounds like a reasonable rule to me. I don't see a corresponding rule in [flake8-datetimez](https://pypi.org/project/flake8-datetimez/), so we would need to decide where to put this functionality. A few options are a new rule in the DTZ namespace, if we think the upstream linter is unlikely to add more rules, a new RUF rule, or as an extension to one of the existing rules.

---
