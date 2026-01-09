---
number: 11090
title: Allowed captalized arg names configuration for N803
type: issue
state: closed
author: Malachov
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-04-22T12:29:40Z
updated_at: 2024-04-23T15:09:05Z
url: https://github.com/astral-sh/ruff/issues/11090
synced_at: 2026-01-07T13:12:15-06:00
---

# Allowed captalized arg names configuration for N803

---

_Issue opened by @Malachov on 2024-04-22 12:29_

# Feature request

## Description
It is common in data science to use capital letters in names (lowercase for vector, uppercase for matrix). Example: [X in fit function in sklearn](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html#sklearn.linear_model.LinearRegression.fit)

This goes against N803, e.g. 'Argument name `X` should be lowercase`'

This leads to many warnings across data science codebases.

I know, that technically it is correct and it should be warned, but to be able to suppress it in `pyproject.toml` could be beneficial in some cases.

## Pylint comparison
Maybe something like [good-names](https://pylint.pycqa.org/en/latest/user_guide/configuration/all-options.html#good-names) could be added. Should it solve only N803? Probably it could whitelist a lot of other rules as well, but I'm not sure.

## Note
If others don't think, that this is good idea, you can close it as maybe there should be issue rather on scikit-learn.

---

_Comment by @charliermarsh on 2024-04-22 16:21_

As a hack, I think you could add it to https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx -- but it's not really the intended use of that variable, and it will ignore other errors on variables named `X`.

I'd be open to adding something like `good-names`, but I'd probably want it to apply to all `N` rules for simplicity. Curious for more opinions though.

\cc'ing @AlexWaygood


---

_Label `configuration` added by @charliermarsh on 2024-04-22 16:21_

---

_Label `needs-decision` added by @charliermarsh on 2024-04-22 16:21_

---

_Comment by @AlexWaygood on 2024-04-23 14:10_

Hmm, we already have these settings that apply to all the `pep8-naming` rules:
- https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names
- https://docs.astral.sh/ruff/settings/#lint_pep8-naming_extend-ignore-names

Do those suffice for this? It seems like they use glob patterns rather than regexes, which is slightly surprising to me, but probably hard to change at this point 😄

---

_Comment by @charliermarsh on 2024-04-23 14:11_

Oh yeah, IMO those suffice.

---

_Comment by @charliermarsh on 2024-04-23 15:09_

I think those settings are sufficient to close this out.

---

_Closed by @charliermarsh on 2024-04-23 15:09_

---
