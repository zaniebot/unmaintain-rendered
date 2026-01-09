---
number: 9473
title: "Is there a way to specify that you don't care about pypy when creating a lockfile?"
type: issue
state: closed
author: wlach
labels:
  - question
  - great writeup
assignees: []
created_at: 2024-11-27T15:25:10Z
updated_at: 2024-11-27T15:53:40Z
url: https://github.com/astral-sh/uv/issues/9473
synced_at: 2026-01-07T13:12:18-06:00
---

# Is there a way to specify that you don't care about pypy when creating a lockfile?

---

_Issue opened by @wlach on 2024-11-27 15:25_

Consider this pyproject.toml:

```toml
[project]
name = "test-clickhouse-driver"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "databricks-sql-connector==3.6.0",
    "clickhouse-driver[lz4]==0.2.9",
]
```

These dependencies are mutually compatible using regular cpython, but not with pypy because `clickhouse-driver[lz4]` specifies that you need an older version of `lz4` (that's incompatible with `databricks-sql-connector`) on that platform:

https://github.com/mymarilyn/clickhouse-driver/blob/8a4e7c5b99b532df2b015651d893a6f36288a22c/setup.py#L131

Thus running `uv sync` fails:

```
❯ uv sync
Using CPython 3.11.9 interpreter at: /Users/wlach/.local/share/mise/installs/python/3.11/bin/python3.11
Creating virtual environment at: .venv
  × No solution found when resolving dependencies for split
  │ (implementation_name == 'pypy'):
  ╰─▶ Because only the following versions of lz4{implementation_name ==
      'pypy'} are available:
          lz4{implementation_name == 'pypy'}==0.1
          lz4{implementation_name == 'pypy'}==0.2
          ...
          lz4{implementation_name == 'pypy'}>=3.0.1
      and databricks-sql-connector==3.6.0 depends on lz4>=4.0.2,
      we can conclude that databricks-sql-connector==3.6.0 and
      lz4{implementation_name == 'pypy'}<=3.0.1 are incompatible.
      And because clickhouse-driver[lz4]==0.2.9 depends on
      lz4{implementation_name == 'pypy'}<=3.0.1, we can conclude that
      databricks-sql-connector==3.6.0 and clickhouse-driver[lz4]==0.2.9 are
      incompatible.
      And because your project depends on clickhouse-driver[lz4]==0.2.9 and
      databricks-sql-connector==3.6.0, we can conclude that your project's
      requirements are unsatisfiable.
```

Locally we can workaround this by adding `;implementation_name != 'pypy'` to the clickhouse driver specification:

```
[project]
name = "test-clickhouse-driver"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "databricks-sql-connector==3.6.0",
    "clickhouse-driver[lz4]==0.2.9;implementation_name != 'pypy'",
]
```

However that doesn't help if one of your dependencies is also depending on the generic `clickhouse-driver[lz4]`. Is there any way I can say that I just don't care about pypy when creating a uv lockfile? I know they're supposed to be cross-platform/implementation but for our use cases at least we don't really need this guarantee.

---

_Comment by @zanieb on 2024-11-27 15:30_

Does a [limited resolution environment](https://docs.astral.sh/uv/concepts/projects/config/#limited-resolution-environments) work for you?

I think it'd be

```toml
[tool.uv]
environments = [
    "implementation_name != 'pypy'"
]
```

---

_Comment by @zanieb on 2024-11-27 15:32_

You can also use [dependency overrides](https://docs.astral.sh/uv/concepts/resolution/#dependency-overrides) to override a transitive dependency

---

_Label `question` added by @zanieb on 2024-11-27 15:32_

---

_Label `great writeup` added by @zanieb on 2024-11-27 15:32_

---

_Comment by @wlach on 2024-11-27 15:35_

> Does a [limited resolution environment](https://docs.astral.sh/uv/concepts/projects/config/#limited-resolution-environments) work for you?
> 
> I think it'd be
> 
> [tool.uv]
> environments = [
>     "implementation_name != 'pypy'"
> ]

Yes! That was exactly what I was looking for. Might make sense to mention pypy there as well?

---

_Referenced in [astral-sh/uv#9475](../../astral-sh/uv/pulls/9475.md) on 2024-11-27 15:43_

---

_Closed by @zanieb on 2024-11-27 15:53_

---

_Closed by @zanieb on 2024-11-27 15:53_

---
