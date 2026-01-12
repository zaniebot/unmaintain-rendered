```yaml
number: 732
title: "ty can't resolve attributes in `msgspec`"
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2025-06-30T22:35:16Z
updated_at: 2025-07-01T00:29:51Z
url: https://github.com/astral-sh/ty/issues/732
synced_at: 2026-01-12T15:54:23Z
```

# ty can't resolve attributes in `msgspec`

---

_@charliermarsh_

For example:

```
error[unresolved-attribute]: Type `<module 'msgspec'>` has no attribute `json`
 --> main.py:3:1
  |
1 | import msgspec
2 |
3 | msgspec.json.decode
  | ^^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

---

_Comment by @carljm on 2025-07-01 00:29_

This is https://github.com/astral-sh/ty/issues/133. It works if you `import msgspec.json` explicitly.

This one I'll close as duplicate, because I think it's pretty straightforwardly captured by the existing issue.

---

_Closed by @carljm on 2025-07-01 00:29_

---
