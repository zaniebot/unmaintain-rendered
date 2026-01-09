---
number: 4061
title: "Support for isort's custom sections and ordering"
type: issue
state: closed
author: robhudson
labels: []
assignees: []
created_at: 2023-04-21T19:02:04Z
updated_at: 2023-04-21T21:55:51Z
url: https://github.com/astral-sh/ruff/issues/4061
synced_at: 2026-01-07T13:12:14-06:00
---

# Support for isort's custom sections and ordering

---

_Issue opened by @robhudson on 2023-04-21 19:02_

Support for [custom sections and ordering](https://pycqa.github.io/isort/docs/configuration/custom_sections_and_ordering.html)

I'd like to use `ruff` to replace `isort` in my project, but currently this is the blocking feature. The project is a Django project and we like to group our Django imports separately from the other 3rd party imports, which `isort` supports via the above.

If this is already possible with `ruff`, it was unclear to me reading the docs and options available. Thanks.

---

_Renamed from "Support for custom sections and ordering" to "Support for isort's custom sections and ordering" by @robhudson on 2023-04-21 19:02_

---

_Comment by @g-as on 2023-04-21 19:41_

This was shipped in the latest release:

https://github.com/charliermarsh/ruff/releases/tag/v0.0.262

---

_Comment by @g-as on 2023-04-21 19:42_

```TOML

[tool.ruff.isort]
known-first-party = ["pouet", "toto"]
extra-standard-library = ["_csv", "_typeshed", "typing_extensions"]
section-order = ["future", "standard-library", "django", "django-contrib", "third-party", "first-party", "tests-deps", "tests", "local-folder"]

[tool.ruff.isort.sections]
django = ["django"]
django-contrib = ["django.contrib"]
tests-deps = ["django.test", "factory", "faker", "pytest", "pytest_django", "respx", "testfixtures", "time_machine", "unittest"]
tests = ["tests"]
```

---

_Comment by @robhudson on 2023-04-21 21:55_

Missed that. Thanks!

---

_Closed by @robhudson on 2023-04-21 21:55_

---
