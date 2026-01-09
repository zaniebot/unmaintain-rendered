---
number: 1004
title: Implement import conventions
type: issue
state: closed
author: edgarrmondragon
labels:
  - rule
assignees: []
created_at: 2022-12-03T01:59:40Z
updated_at: 2022-12-06T21:01:18Z
url: https://github.com/astral-sh/ruff/issues/1004
synced_at: 2026-01-07T13:12:14-06:00
---

# Implement import conventions

---

_Issue opened by @edgarrmondragon on 2022-12-03 01:59_

There's https://github.com/joaopalmeiro/flake8-import-conventions to serve as inspiration, though I'm not sure if I'd like to replicate the behavior 100%.

It has an error code for each convention:

```python
name2asname: Dict[str, str] = {
    "altair": "alt",   # IC001
    "numpy": "np",     # IC002
    "pandas": "pd",    # IC003
    "seaborn": "sns",  # IC004
}
```

The biggest problem with this approach is that it's not possible to expand with custom conventions without forking the repo.

One alternative would be to use a single error code for all conventions, default to the ones above, and allow users to configure a custom mapping:

```toml
[tool.ruff.import_conventions.aliases]
pulumi_aws = "aws"
```

---

_Label `rule` added by @charliermarsh on 2022-12-03 22:02_

---

_Comment by @charliermarsh on 2022-12-03 22:02_

Yeah I like what you outlined above: one rule ("Invalid alias" or whatever), with some defaults, that can be overridden.

---

_Comment by @edgarrmondragon on 2022-12-05 22:53_

Cool, I'll work on the implementation then 😄 

---

_Referenced in [astral-sh/ruff#1098](../../astral-sh/ruff/pulls/1098.md) on 2022-12-06 03:06_

---

_Closed by @charliermarsh on 2022-12-06 21:01_

---
