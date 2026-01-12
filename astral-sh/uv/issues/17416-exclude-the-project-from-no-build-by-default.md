```yaml
number: 17416
title: "Exclude the project from `no-build` by default"
type: issue
state: open
author: NMertsch
labels:
  - enhancement
assignees: []
created_at: 2026-01-12T13:43:52Z
updated_at: 2026-01-12T13:44:43Z
url: https://github.com/astral-sh/uv/issues/17416
synced_at: 2026-01-12T14:04:12Z
```

# Exclude the project from `no-build` by default

---

_Issue opened by @NMertsch on 2026-01-12 13:43_

### Summary

Currently, package projects (`uv init --package`) automatically are added to `uv.lock` as editable. This makes sense.
But it also means that `no-build = true` applies to them, and not just to dependencies.

I'd like to have the following:
1. External dependencies are not build, unless explicitly allowed with `no-binary-package`
2. My project is built

Point 1 is achieved with `no-build = true`. I think it is reasonable to expect point 2 without additional configuration.

### How to reproduce
```bash
# create new package
uv init --package my-project
cd my-project

# set `no-build = true`
echo '[tool.uv]' >> pyproject.toml
echo 'no-build = true' >> pyproject.toml

# interact with the package
uv sync  # error: Distribution `my-project==0.1.1 @ editable+.` can't be installed because it is marked as `--no-build` but has no binary distribution

# explicitly allow uv to build my own package
echo 'no-binary-package' = ["my-project"] >> pyproject.toml

# interact with the package
uv sync  # works
```

---

_Label `enhancement` added by @NMertsch on 2026-01-12 13:43_

---
