---
number: 14483
title: "Support `--all-groups` for `uv pip install`"
type: issue
state: open
author: mikeweltevrede
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-07-07T09:22:18Z
updated_at: 2025-12-16T07:54:10Z
url: https://github.com/astral-sh/uv/issues/14483
synced_at: 2026-01-07T13:12:18-06:00
---

# Support `--all-groups` for `uv pip install`

---

_Issue opened by @mikeweltevrede on 2025-07-07 09:22_

### Summary

Support `--all-groups` for `uv pip install` just like with `uv sync`. Right now, all groups need to be manually provided. This is overly verbose while there is already precedent for an `--all-groups` argument.

Could not find any related issues in the issue tracker with search `uv pip install all-groups`: https://github.com/astral-sh/uv/issues?q=is%3Aissue%20%20sort%3Arelevance-desc%20uv%20pip%20install%20all-groups

### Example

Instead of:
```shell
uv pip install -r pyproject.toml --all-extras --group=dev --group=lint --strict
```

one would use 
```shell
uv pip install -r pyproject.toml --all-extras --all-groups --strict
```

---

_Label `enhancement` added by @mikeweltevrede on 2025-07-07 09:22_

---

_Label `help wanted` added by @zanieb on 2025-07-11 12:52_

---

_Comment by @zanieb on 2025-07-11 12:53_

This seems reasonable to me.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-20 22:47_

---

_Referenced in [astral-sh/uv#14789](../../astral-sh/uv/pulls/14789.md) on 2025-07-21 14:32_

---

_Comment by @mikeweltevrede on 2025-12-16 07:54_

Hi @charliermarsh @zanieb, I hope you are doing well. Seeing as there is a PR open, I was wondering when we can expect this functionality to be included. Thanks so much and happy holidays in advance!

---
