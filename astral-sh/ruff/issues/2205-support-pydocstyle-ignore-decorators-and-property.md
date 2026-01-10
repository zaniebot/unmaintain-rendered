```yaml
number: 2205
title: "Support pydocstyle `ignore_decorators` and `property_decorators` options"
type: issue
state: closed
author: edgarrmondragon
labels:
  - good first issue
  - configuration
assignees: []
created_at: 2023-01-26T17:29:26Z
updated_at: 2023-03-02T22:00:12Z
url: https://github.com/astral-sh/ruff/issues/2205
synced_at: 2026-01-10T11:09:45Z
```

# Support pydocstyle `ignore_decorators` and `property_decorators` options

---

_Issue opened by @edgarrmondragon on 2023-01-26 17:29_

- [x] `ignore-decorators`: ignore any functions or methods that are decorated by a function with a name fitting the `<decorators>` regular expression (https://github.com/charliermarsh/ruff/pull/3229)
- [x] `property-decorators`: consider any method decorated with one of these decorators as a property, and consequently allow a docstring which is not in imperative mood

---

_Comment by @charliermarsh on 2023-01-26 17:30_

Should `pycodestyle` in the issue name be `pydocstyle`?

---

_Comment by @edgarrmondragon on 2023-01-26 17:33_

> Should `pycodestyle` in the issue name be `pydocstyle`?

Yup 😅. Updated

---

_Renamed from "Support pycodestyle `ignore_decorators` and `property_decorators` options" to "Support pydocstyle `ignore_decorators` and `property_decorators` options" by @edgarrmondragon on 2023-01-26 17:33_

---

_Label `configuration` added by @charliermarsh on 2023-01-26 22:32_

---

_Label `good first issue` added by @charliermarsh on 2023-02-03 00:13_

---

_Comment by @charliermarsh on 2023-02-03 04:45_

I think we should probably do this not via regex, but via module identifiers, for consistency with other settings.

As an example, this:

```toml
[tool.ruff.pydocstyle]
ignore-decorators = ["foo.bar.baz"]
```

Would ignore `@foo.bar.baz`, or `from foo.bar import baz; @baz`, etc.

---

_Comment by @edgarrmondragon on 2023-02-25 21:38_

I'm working on `ignore-decorators`

---

_Comment by @staticssleever668 on 2023-03-02 19:30_

I'll work on `property-decorators`

---

_Comment by @charliermarsh on 2023-03-02 20:40_

@staticssleever668 - Awesome, thank you! https://github.com/charliermarsh/ruff/pull/3229 is probably a good reference, hopefully helpful.

---

_Closed by @charliermarsh on 2023-03-02 22:00_

---
