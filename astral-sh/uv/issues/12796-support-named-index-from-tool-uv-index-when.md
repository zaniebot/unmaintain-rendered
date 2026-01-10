---
number: 12796
title: "Support named index from `tool.uv.index` When running the `uv add` command"
type: issue
state: closed
author: EATSTEAK
labels:
  - enhancement
assignees: []
created_at: 2025-04-10T01:47:31Z
updated_at: 2025-04-10T14:03:38Z
url: https://github.com/astral-sh/uv/issues/12796
synced_at: 2026-01-10T01:25:25Z
---

# Support named index from `tool.uv.index` When running the `uv add` command

---

_Issue opened by @EATSTEAK on 2025-04-10 01:47_

### Summary

Currently, the `uv add` command only supports custom indexes by specifying the full URL.
This proposal suggests adding support for named indexes defined in the `tool.uv.index` section, so users don’t have to provide the full index URL every time.

To achieve this, a new `--index-name` parameter would be introduced for `uv add` and other index-related commands.

### Example

```toml
# in pyproject.toml
[[tool.uv.index]]
name = "custom"
url = "https://custom.pypi.index/pypi/simple/"
explicit = true
```

Select a `custom` index using only its name:

**AS-IS**
```bash
uv add custom-dependency --index custom=https://custom.pypi.index/pypi/simple/
```

**TO-BE**
```bash
uv add custom-dependency --index-name custom
```

---

_Label `enhancement` added by @EATSTEAK on 2025-04-10 01:47_

---

_Comment by @jtfmumm on 2025-04-10 10:25_

What behavior do you have in mind when you supply the index name in this way? Currently, uv will already use the indexes defined in `pyproject.toml` when resolving dependencies. See [here](https://docs.astral.sh/uv/configuration/indexes/#defining-an-index) and [here](https://docs.astral.sh/uv/configuration/indexes/#searching-across-multiple-indexes) in the docs. 

Are you looking for a way to specify a `pyproject.toml` configured index at the command line as the only index to search? 

---

_Comment by @charliermarsh on 2025-04-10 14:03_

I believe this is the same as https://github.com/astral-sh/uv/issues/10140.

---

_Closed by @charliermarsh on 2025-04-10 14:03_

---

_Comment by @charliermarsh on 2025-04-10 14:03_

(We def want to support this.)

---

_Referenced in [astral-sh/uv#10140](../../astral-sh/uv/issues/10140.md) on 2025-08-01 10:48_

---
