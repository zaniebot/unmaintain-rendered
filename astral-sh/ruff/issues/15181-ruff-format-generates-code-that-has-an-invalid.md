```yaml
number: 15181
title: Ruff format generates code that has an invalid syntax in Python 3.8
type: issue
state: closed
author: bblommers
labels:
  - question
  - formatter
assignees: []
created_at: 2024-12-29T13:14:05Z
updated_at: 2024-12-30T04:24:30Z
url: https://github.com/astral-sh/ruff/issues/15181
synced_at: 2026-01-12T15:54:54Z
```

# Ruff format generates code that has an invalid syntax in Python 3.8

---

_@bblommers_

If you have a `with` statement with two context managers with fairly long name, running `ruff format` will put them on a separate line:
```
with (
    GenericContextManagerWithAFairlyLongName() as some_volume,
    GenericContextManagerWithAFairlyLongName() as gcm2,
):
```

I don't quite understand why, but this is considered invalid syntax in Python 3.8.

A reproduction can be found here:
https://github.com/bblommers/ruff-py3.8-compat/pull/2

The original source is included in the PR.

The CI workflow formats it using `ruff`, and you can see that the invocation of the original file succeeds - but invoking the formatted file fails in Python 3.7/3.8:

https://github.com/bblommers/ruff-py3.8-compat/actions/runs/12535710649/job/34957895029?pr=2

---

_Comment by @wookie184 on 2024-12-29 14:32_

If you set the target version (https://docs.astral.sh/ruff/settings/#target-version) to 3.8 it should fix this.

Parentheses here are only supported in 3.9+ because the syntax wasn't possible to implement with the old parser (https://peps.python.org/pep-0617/ if you're interested)

---

_Comment by @bblommers on 2024-12-29 17:21_

Our project does have the `python_requires` metadata set, but in `setup.cfg` instead of in `pyproject.toml` - I guess that's why `ruff` doesn't pick it up.

And TIL about the PEP! It actually specifically mentions this example 🙂

Thank you for the info @wookie184!

---

_Closed by @bblommers on 2024-12-29 17:21_

---

_Label `question` added by @dhruvmanila on 2024-12-30 04:24_

---

_Label `formatter` added by @dhruvmanila on 2024-12-30 04:24_

---
