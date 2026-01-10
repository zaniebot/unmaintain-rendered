```yaml
number: 12703
title: "PT006: Could csv mode have a space between names?"
type: issue
state: open
author: notatallshaw
labels:
  - configuration
assignees: []
created_at: 2024-08-05T23:32:21Z
updated_at: 2024-08-12T05:51:22Z
url: https://github.com/astral-sh/ruff/issues/12703
synced_at: 2026-01-10T11:09:54Z
```

# PT006: Could csv mode have a space between names?

---

_Issue opened by @notatallshaw on 2024-08-05 23:32_

I am working on adding [pytest rules for pip](https://github.com/pypa/pip/pull/12894). Turning on [PT006](https://docs.astral.sh/ruff/rules/pytest-parametrize-names-wrong-type/) with [lint.flake8-pytest-style.parametrize-names-type](https://docs.astral.sh/ruff/settings/#lint_flake8-pytest-style_parametrize-names-type) set to csv produces a slight styling difference on autofix of tests using tuple or list compared to pip's standard style.

It produces "[one,two,three](https://github.com/pypa/pip/pull/12894#discussion_r1703582965)" instead of "[one, two, three](https://github.com/pypa/pip/blob/3259c061fb36698339877bbf2cefb5e82815e812/tests/functional/test_install_extras.py#L198)". 

Would any of the following requests be considered?

* The autofix of tuple or list to csv converts to `one, two, three, four` not `one,two,three,four`
* This be the standard style so any csv string, e.g. `one,two  ,three,  four` gets neatly formatted to `one, two, three, four`
* An additional option to enforce either of these



---

_Comment by @charliermarsh on 2024-08-05 23:34_

I'm personally fine with all of those changes, but I'm not super familiar with the norms here so would be nice to have others chime in :)

---

_Label `configuration` added by @charliermarsh on 2024-08-06 02:30_

---

_Comment by @MichaReiser on 2024-08-06 05:49_

Is there a way we could infer the preferred style, e.g. by preserving the existing spacing (similar to stylist inferring the line ending from the first use line ending)

---

_Comment by @RonnyPfannschmidt on 2024-08-06 06:55_

I recommend adding the spaces for reading help

---

_Comment by @Avasam on 2024-08-12 05:51_

`"one, two, three"` is also closer in style to what you'd get with list or tuple (ie: `["one", "two", "three"]`) from the formatter.

---
