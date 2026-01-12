```yaml
number: 12477
title: How to automatically choose the available index to use for installing dependancies?
type: issue
state: open
author: codgas
labels:
  - question
assignees: []
created_at: 2025-03-26T03:58:07Z
updated_at: 2025-04-01T19:00:13Z
url: https://github.com/astral-sh/uv/issues/12477
synced_at: 2026-01-12T16:01:04Z
```

# How to automatically choose the available index to use for installing dependancies?

---

_@codgas_

### Question

I have a single `pyproject.toml` that specifies multiple indexes and dependency groups, for example:

```
dependencies = [ ... ]

[dependency-groups]
group_a = [...]
group_b = [...]

[tool.uv.sources]
 group_a = [{ index = "index1" }]
 group_b = [{ index = "index2" }]

[[tool.uv.index]]
name = "index1"
url = "https://url1/"

[[tool.uv.index]]
name = "index2"
url = "https://url2/"
```

I have different Docker builds targeting different environments (group_a vs. group_b). The group_a environment cannot access 'index2', while group_b cannot access the other ('index1'). 
However, when I run:
 `uv sync  --group_b `
It chooses index1 to install dependencies (which group_b can’t access), and fails as a result.

I’d like uv to automatically pick the correct (accessible) index or gracefully ignore the one that’s unavailable—ideally without large code changes or fully splitting my pyproject.toml. While I could move dependencies into separate group-level dependency blocks, that would add redundant declarations, which I’d prefer to avoid.
Anyone have any idea how to do this ? 

### Platform

Debian

### Version

0.6.9

---

_Label `question` added by @codgas on 2025-03-26 03:58_

---

_Comment by @sanmai-NL on 2025-03-26 08:44_

- https://docs.astral.sh/uv/reference/settings/#pip_index-strategy
- https://docs.astral.sh/uv/reference/settings/#index (`explicit` and `default`)

---

_Comment by @codgas on 2025-03-26 09:10_

@sanmai-NL 
Thanks for the advice, sadly the url is completely inaccessible so even with changing  the index strategy, it kept giving 
`error: Failed to fetch:` url

---

_Comment by @zanieb on 2025-04-01 19:00_

I think this is loosely a duplicate of https://github.com/astral-sh/uv/issues/6349

cc @jtfmumm 

---

_Assigned to @jtfmumm by @zanieb on 2025-04-01 19:00_

---
