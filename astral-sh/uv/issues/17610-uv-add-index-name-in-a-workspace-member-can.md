```yaml
number: 17610
title: "`uv add --index <name>=...` in a workspace member can change existing dependencies' index"
type: issue
state: open
author: EliteTK
labels:
  - bug
assignees: []
created_at: 2026-01-19T14:07:32Z
updated_at: 2026-01-19T14:07:32Z
url: https://github.com/astral-sh/uv/issues/17610
synced_at: 2026-01-19T14:23:30Z
```

# `uv add --index <name>=...` in a workspace member can change existing dependencies' index

---

_@EliteTK_

### Summary

In a workspace with a `pyproject.toml` containing an index definition it is possible to reference this definition from the children of that workspace.

When adding a new dependency for a child with a specified named index, the child's `pyproject.toml` is automatically updated to include this new definition.

It's possible to add a new dependency that uses a new named index. This index will be added into the child's `pyproject.toml`. If this index bears a name which was already defined in the parent workspace, this will shadow the parent's definition for all dependencies of that child.

This means that if another dependency was already using the parent's index definition, it will now be automatically switched over to using the child's.

We already overwrite existing indexes in a `pyproject.toml` if you provide a new one with the same name. Thus this doesn't seem that serious. But I have to say it's somewhat surprising.

Reproducer:

```bash
#!/usr/bin/env bash

set -e

tempdir=$(mktemp -d)
trap 'rm -rf "$tempdir"' EXIT

cat <<EOF >"$tempdir/pyproject.toml"
[project]
name = "parent"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = []

[tool.uv.workspace]
members = ["child"]

[[tool.uv.index]]
name = "test-index"
url = "https://test.pypi.org/simple"
EOF

mkdir -p -- "$tempdir/child"
cat <<EOF >"$tempdir/child/pyproject.toml"
[project]
name = "child"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = [
    "requests",
]

[tool.uv.sources]
requests = { index = "test-index" }
EOF

uv --directory "$tempdir" sync

cp "$tempdir/pyproject.toml"{,.initial}
cp "$tempdir/child/pyproject.toml"{,.initial}
cp "$tempdir/uv.lock"{,.initial}

set -x
uv --directory "$tempdir" add --index test-index=https://pypi.org/simple --package child flask
set +x

set +e
diff -ur --color=auto "$tempdir/pyproject.toml"{.initial,}
diff -ur --color=auto "$tempdir/child/pyproject.toml"{.initial,}
diff -ur --color=auto "$tempdir/uv.lock"{.initial,}
set -e
```

### Platform

Linux 6.12.63_1 x86_64 GNU/Linux

### Version

uv 0.9.26

### Python version

_No response_

---

_Label `bug` added by @EliteTK on 2026-01-19 14:07_

---
