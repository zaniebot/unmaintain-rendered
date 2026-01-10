```yaml
number: 14077
title: WIP unused sources warning
type: pull_request
state: open
author: oconnor663
labels: []
assignees: []
draft: true
base: main
head: jack/sources_warning
created_at: 2025-06-16T14:59:18Z
updated_at: 2025-06-16T14:59:18Z
url: https://github.com/astral-sh/uv/pull/14077
synced_at: 2026-01-10T11:10:42Z
```

# WIP unused sources warning

---

_Pull request opened by @oconnor663 on 2025-06-16 14:59_

https://github.com/astral-sh/uv/issues/13829

```toml
[project]
name = "scratch"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "pandas>=2.3.0",
]

[[tool.uv.sources.numpy]]
path = "/var/tmp/numpy-2.2.0-cp313-cp313-manylinux_2_17_x86_64.manylinux2014_x86_64.whl"
```

```
$ uv sync
warning: some [tool.uv.sources] don't match a direct dependency and have no effect: numpy
Resolved 11 packages in 3ms
Audited 7 packages in 0.23ms
```

```diff
diff --git i/pyproject.toml w/pyproject.toml
index 9e673b0..0b9ebbb 100644
--- i/pyproject.toml
+++ w/pyproject.toml
@@ -3,6 +3,7 @@ name = "scratch"
 version = "0.1.0"
 requires-python = ">=3.13"
 dependencies = [
+    "numpy>=2.2.0",
     "pandas>=2.3.0",
 ]
```
```
$ fastuv sync
Resolved 7 packages in 2ms
Audited 6 packages in 0.05ms
```

This is the most direct way I could find to get this warning to work, but it's clearly incomplete. There are TODO's there for workspace sources, build-requires, and dependency groups, all of which need to be taken into account. It looks like the codepaths into these different areas are totally different, and I'll probably need to take a different approach overall. Putting this draft up to make it easier to talk this over with folks who are more familiar.

---
