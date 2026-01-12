```yaml
number: 6828
title: "`uvx --from` doesn't respect upper bounds"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-08-29T18:56:56Z
updated_at: 2024-08-29T18:57:49Z
url: https://github.com/astral-sh/uv/issues/6828
synced_at: 2026-01-12T15:59:08Z
```

# `uvx --from` doesn't respect upper bounds

---

_@konstin_

`uvx` ignores bounds passed to `--from`. From the docs, i couldn't figure out how to install an older version without using an exact version.

```
$ uvx --from "uv>=0.2,<3" uv --version
uv 0.4.0
```

---

_Label `bug` added by @konstin on 2024-08-29 18:57_

---

_Comment by @konstin on 2024-08-29 18:57_

-.- i meant `0.3` 

---

_Closed by @konstin on 2024-08-29 18:57_

---
