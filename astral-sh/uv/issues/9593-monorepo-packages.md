---
number: 9593
title: Monorepo packages
type: issue
state: closed
author: Dayof
labels:
  - question
assignees: []
created_at: 2024-12-03T03:39:58Z
updated_at: 2024-12-03T05:03:45Z
url: https://github.com/astral-sh/uv/issues/9593
synced_at: 2026-01-10T01:24:43Z
---

# Monorepo packages

---

_Issue opened by @Dayof on 2024-12-03 03:39_

I have this following structure on poetry:

```
[tool.poetry]
packages = [
  { include = "lib-io", from = "src/io" },
  { include = "transforms", from = "src/transforms" }
]
```

When I install my library, the io module is available when I try to import, example:


```
python -c "import lib_io"
```

Is it possible to do the same on uv?

I tried to do the following:

```
[tool.uv.sources]
lib-io = { path = "src/io" }
```

But it says that there is no module `lib-io`:

```
uv run python -c "import lib_io"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'lib_io'
```

Note:
- I cannot add them as workspaces since they have dependencies conflicts.

---

_Comment by @samypr100 on 2024-12-03 03:52_

I think if you're using hatch as your build backend you can accomplish this via `tool.hatch.build.targets.wheel`

e.g.

```
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["src/io", "src/transforms"]
````

---

_Label `question` added by @samypr100 on 2024-12-03 03:53_

---

_Comment by @Dayof on 2024-12-03 05:03_

It worked! Thanks for the fast assistance :)

---

_Closed by @Dayof on 2024-12-03 05:03_

---
