---
number: 10155
title: "Suggestion to add `any` to possible values of `--python-platform` parametr"
type: issue
state: open
author: oliversen
labels:
  - question
  - configuration
assignees: []
created_at: 2024-12-25T16:42:51Z
updated_at: 2025-01-01T20:07:52Z
url: https://github.com/astral-sh/uv/issues/10155
synced_at: 2026-01-10T01:24:50Z
---

# Suggestion to add `any` to possible values of `--python-platform` parametr

---

_Issue opened by @oliversen on 2024-12-25 16:42_

I suggest adding `any` to the possible values ​​of the `--python-platform` parameter to support installing [Pure Python Wheels](https://packaging.python.org/en/latest/guides/distributing-packages-using-setuptools/#pure-python-wheels).

---

_Comment by @charliermarsh on 2024-12-26 14:34_

Is the suggestion that `any` would _require_ Pure Python wheels, and reject any platform-specific wheels?

---

_Label `question` added by @charliermarsh on 2024-12-26 14:34_

---

_Label `configuration` added by @charliermarsh on 2024-12-26 14:34_

---

_Comment by @oliversen on 2025-01-01 20:00_

Yes. Just like pip does. For example:

```shell
pip install pydantic -t bundled\libs --only-binary :all: --platform any

...
Successfully installed pydantic-1.10.19 typing-extensions-4.12.2
```

Installed the latest available version of the package having Pure Python Wheels.
But if you specify a package version that doesn't have Pure Python Wheels (or its dependencies), an error will occur:

```shell
pip install "pydantic>=2.0" -t bundled\libs --only-binary :all: --platform any

...
ERROR: Cannot install pydantic==2.0, pydantic==2.0.1, ...  and pydantic==2.9.2 because these package versions have conflicting dependencies.
```


---

_Comment by @charliermarsh on 2025-01-01 20:07_

That makes sense, thanks.

---
