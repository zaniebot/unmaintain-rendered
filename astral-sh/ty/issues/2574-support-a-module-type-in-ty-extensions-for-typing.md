```yaml
number: 2574
title: "support a `Module` type in `ty_extensions` for typing specific modules"
type: issue
state: open
author: KotlinIsland
labels: []
assignees: []
created_at: 2026-01-21T01:57:23Z
updated_at: 2026-01-21T01:57:30Z
url: https://github.com/astral-sh/ty/issues/2574
synced_at: 2026-01-21T03:00:14Z
```

# support a `Module` type in `ty_extensions` for typing specific modules

---

_@KotlinIsland_

### Summary

```py
from ty_extensions import Module


def get_module() ->  Module["collections.abc"]:
    from collections import abc
    
    return abc
```

---
