---
number: 12334
title: "[documentation] `project.dependencies` is not a TOML table"
type: issue
state: closed
author: andrei-korshikov
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2025-03-20T09:53:22Z
updated_at: 2025-03-23T03:06:12Z
url: https://github.com/astral-sh/uv/issues/12334
synced_at: 2026-01-10T01:25:18Z
---

# [documentation] `project.dependencies` is not a TOML table

---

_Issue opened by @andrei-korshikov on 2025-03-20 09:53_

[Project dependencies](https://docs.astral.sh/uv/concepts/projects/dependencies/#project-dependencies) reads:

> The `project.dependencies` table represents the dependencies that are used when uploading to PyPI or building a wheel.

[PEP 621](https://peps.python.org/pep-0621/#dependencies-optional-dependencies) reads:

> Format: Array of [PEP 508](https://peps.python.org/pep-0508/) strings (`dependencies`) and a table with values of arrays of [PEP 508](https://peps.python.org/pep-0508/) strings (`optional-dependencies`)

So, to my understanding, `project.dependencies` is an array (kinda list), not a table (map, dictionary, key-value pairs).


---

_Comment by @andrei-korshikov on 2025-03-20 10:13_

Hmm, then [Dependency tables](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-tables) are not "tables" but "array and tables"…

---

_Comment by @andrei-korshikov on 2025-03-20 10:29_

[Dependency tables](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-tables) reads:

> The `tool.uv.sources` table extends the standard dependency tables

and has `dependencies = ["foo"]` array sample.

Am I missing something obvious?

---

_Comment by @zanieb on 2025-03-20 13:14_

We could probably just switch to "field" or something here. I don't think "table" was intended to convey semantic meaning as used.

---

_Label `documentation` added by @zanieb on 2025-03-20 13:14_

---

_Label `good first issue` added by @zanieb on 2025-03-20 13:14_

---

_Referenced in [astral-sh/uv#12388](../../astral-sh/uv/pulls/12388.md) on 2025-03-22 09:32_

---

_Closed by @zanieb on 2025-03-23 03:06_

---

_Closed by @zanieb on 2025-03-23 03:06_

---
