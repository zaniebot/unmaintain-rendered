---
number: 3034
title: "Feature request: add annotation for dependency artifact URL"
type: issue
state: open
author: vlad-ivanov-name
labels:
  - wish
assignees: []
created_at: 2024-04-15T10:23:24Z
updated_at: 2024-04-20T01:27:22Z
url: https://github.com/astral-sh/uv/issues/3034
synced_at: 2026-01-07T13:12:17-06:00
---

# Feature request: add annotation for dependency artifact URL

---

_Issue opened by @vlad-ivanov-name on 2024-04-15 10:23_

It would be good to be able to add URL to the compiled requirement files:

```
uv pip compile --emit-url-annotation ...
```

```
zipp==3.15.0
    # via importlib-metadata
    # from https://pypi.org/simple
    # url https://files.pythonhosted.org/packages/5b/fa/c9e82bbe1af6266adf08afb563905eb87cab83fde00a0a08963510621047/zipp-3.15.0-py3-none-any.whl
```

### Use case

For certain systems consuming compiled requirement files, such as e. g. some nix setups, it would be much easier if files could be fetched directly from URL, as opposed to actually needing to support registry semantics

### Caveats

Maybe there are some package registries that don't have deterministic URLs? Not aware of any atm

---

_Label `wish` added by @charliermarsh on 2024-04-20 01:27_

---

_Referenced in [astral-sh/uv#2679](../../astral-sh/uv/issues/2679.md) on 2024-04-22 04:23_

---
