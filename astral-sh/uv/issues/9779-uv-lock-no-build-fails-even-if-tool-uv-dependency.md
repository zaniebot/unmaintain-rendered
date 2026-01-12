```yaml
number: 9779
title: "`uv lock --no-build` fails even if `tool.uv.dependency-metadata` is set"
type: issue
state: closed
author: konstin
labels:
  - needs-decision
assignees: []
created_at: 2024-12-10T16:10:14Z
updated_at: 2025-02-01T19:05:15Z
url: https://github.com/astral-sh/uv/issues/9779
synced_at: 2026-01-12T15:59:58Z
```

# `uv lock --no-build` fails even if `tool.uv.dependency-metadata` is set

---

_@konstin_

We intended `tool.uv.dependency-metadata` as an escape hatch for packages that can't or shouldn't be built at resolve time, but currently, we treat packages with a `tool.uv.dependency-metadata` entry just like other non-static-metadata source distributions in `uv lock --no-build`.

Instead, we should allow statically specified metadata in `--no-build` resolutions.

```toml
[project]
name = "pkg"
version = "0.1.0"
requires-python = "==3.13.*"
dependencies = ["unicodecsv==0.14.1"]

[[tool.uv.dependency-metadata]]
name = "unicodecsv"
version = "0.14.1"
requires-dist = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

```
$ uv lock --no-build
  × No solution found when resolving dependencies:
  ╰─▶ Because unicodecsv==0.14.1 has no usable wheels and building from source is disabled and your project depends on unicodecsv==0.14.1, we can conclude that your project's requirements
      are unsatisfiable.
```

---

_Label `bug` added by @konstin on 2024-12-10 16:10_

---

_Comment by @charliermarsh on 2024-12-10 16:16_

Thanks. I can look into this today.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-10 19:08_

---

_Comment by @charliermarsh on 2024-12-10 19:15_

I'm now a bit unsure about this one, in part because I think we use this to backtrack? So if `--no-build` is specified, we might backtrack to versions that have wheels available.


---

_Label `needs-decision` added by @charliermarsh on 2024-12-10 19:15_

---

_Label `bug` removed by @charliermarsh on 2024-12-11 03:42_

---

_Closed by @charliermarsh on 2025-02-01 19:05_

---
