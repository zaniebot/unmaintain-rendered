```yaml
number: 12450
title: "Explain what `default = true` means in the configuration docs"
type: issue
state: closed
author: mcrumiller
labels:
  - bug
assignees: []
created_at: 2025-03-24T20:06:41Z
updated_at: 2025-03-24T21:29:27Z
url: https://github.com/astral-sh/uv/issues/12450
synced_at: 2026-01-12T16:01:03Z
```

# Explain what `default = true` means in the configuration docs

---

_@mcrumiller_

### Summary

In https://docs.astral.sh/uv/configuration/files/, the sample configuration for an index-url is

```toml
[[tool.uv.index]]
url = "https://test.pypi.org/simple"
default = true
```

There is no explanation what `default = true` means. Is that required to use the index? Why?

---

_Label `bug` added by @mcrumiller on 2025-03-24 20:06_

---

_Comment by @herebebeasties on 2025-03-24 21:14_

The page you link explains how configuration _files_ work as opposed to being a detailed enumeration and explanation of all possible options in the configuration file.
Indexes are explained in detail in their own dedicated page: https://docs.astral.sh/uv/configuration/indexes/#defining-an-index

---

_Comment by @mcrumiller on 2025-03-24 21:29_

Thanks @herebebeasties.

---

_Closed by @mcrumiller on 2025-03-24 21:29_

---
