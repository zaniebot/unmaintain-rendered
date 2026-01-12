```yaml
number: 7237
title: "How to `uv add` with markers?"
type: issue
state: closed
author: dimaqq
labels:
  - documentation
  - question
assignees: []
created_at: 2024-09-10T00:53:30Z
updated_at: 2025-05-12T16:20:15Z
url: https://github.com/astral-sh/uv/issues/7237
synced_at: 2026-01-12T15:59:11Z
```

# How to `uv add` with markers?

---

_@dimaqq_

Let's say I want to achieve `tomli = { version = "~2.0", python = "<3.11" }`.

How do I `uv add` it? I just can't figure out the syntax and docs don't help.

If manual edit of pyproject.toml is required, maybe there should be a separate page in the docs.


---

_Comment by @zanieb on 2024-09-10 02:44_

Like this

```
❯ uv add --directory example "tomli>=2.0; python_version < '3.11'"

❯ cat example/pyproject.toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "tomli>=2.0 ; python_full_version < '3.11'",
]
```

---

_Label `documentation` added by @zanieb on 2024-09-10 02:45_

---

_Label `question` added by @zanieb on 2024-09-10 02:45_

---

_Comment by @zanieb on 2024-09-10 02:45_

cc @konstin / @charliermarsh should we support adding markers via a dedicated flag (or flags)?

---

_Comment by @phi-friday on 2024-09-11 03:38_

Isn't it enough to point users to links?
like
https://peps.python.org/pep-0508/
https://peps.python.org/pep-0621/
https://packaging.python.org/en/latest/specifications/dependency-specifiers/

Of course, it would be nice to have more configurable options.

---

_Comment by @dimaqq on 2024-09-11 04:20_

- `uv add --help` mentions PEP 508, which includes:
   - an example under the Examples heading, but it's unclear how to convert that to a single command line argument
   - a set of tests at the end of the PEP, that clarifies syntax, esp. quotation
- PEP 621 was not useful for me
- packaging dependency specifiers seems to be a copy of PEP 508

I personally would be very happy if the `uv add` usage had a short link to **uv docs** as opposed to naming PEP, and the uv docs contained additional examples.

---

_Assigned to @zanieb by @zanieb on 2024-10-21 21:48_

---

_Comment by @jedahan on 2025-05-12 16:18_

Flag exists! Not sure if the docs show, but this worked for me:

    uv add pywhispercpp --marker "sys_platform == 'darwin'"

---

_Closed by @charliermarsh on 2025-05-12 16:20_

---
