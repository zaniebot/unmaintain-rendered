```yaml
number: 2574
title: "support a `Module` type in `ty_extensions` for typing specific modules"
type: issue
state: open
author: KotlinIsland
labels: []
assignees: []
created_at: 2026-01-21T01:57:23Z
updated_at: 2026-01-21T08:24:48Z
url: https://github.com/astral-sh/ty/issues/2574
synced_at: 2026-01-21T09:02:45Z
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

_Comment by @AlexWaygood on 2026-01-21 08:24_

Can you give a more full example of a situation in which you might use this? :-) are you thinking that you might use this to annotate functions that lazy-load expensive modules to reduce import times?

---
