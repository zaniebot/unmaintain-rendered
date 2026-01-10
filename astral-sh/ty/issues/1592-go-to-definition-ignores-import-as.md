```yaml
number: 1592
title: "Go to definition ignores `import as`"
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-11-19T13:29:22Z
updated_at: 2025-11-21T19:54:36Z
url: https://github.com/astral-sh/ty/issues/1592
synced_at: 2026-01-10T01:58:59Z
```

# Go to definition ignores `import as`

---

_Issue opened by @MichaReiser on 2025-11-19 13:29_

### Summary

The title isn't great but the case I'm seeing:

`foo/bar.py`

```py
ALL = [1, 2, 3]

```

`main.py`

```py
from bar import ALL as all


print(all)
```

I'd expect that clicking on `all` in `print(all)` jumps to `from bar import ALL as all` but ty directly jumps to `ALL` in `foo/bar.py`

https://play.ty.dev/34121ab8-f6d3-40d6-bcb5-6eb82f48945f

### Version

_No response_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-19 13:29_

---

_Label `server` added by @MichaReiser on 2025-11-19 13:29_

---

_Comment by @Gankra on 2025-11-21 19:54_

I believe this is because `goto_definition` hardcodes `ImportAliasResolution::ResolveAliases`

---
