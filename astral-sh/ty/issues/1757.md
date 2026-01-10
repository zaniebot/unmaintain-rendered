```yaml
number: 1757
title: "`object.attr` completions should respect `__all__` more strictly"
type: issue
state: open
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-04T15:42:37Z
updated_at: 2025-12-04T15:42:37Z
url: https://github.com/astral-sh/ty/issues/1757
synced_at: 2026-01-10T01:56:41Z
```

# `object.attr` completions should respect `__all__` more strictly

---

_Issue opened by @BurntSushi on 2025-12-04 15:42_

Consider this `main.py`:

```
import bar
bar.ZQ<CURSOR>
```

And this `bar.py`:

```
ZQZQ1 = 1
ZQZQ2 = 1
__all__ = ['ZQZQ1']
```

https://github.com/astral-sh/ruff/pull/21779 made auto-import _strictly_ respect `__all__`. But our `object.attr` completions are looser and still expose symbols that aren't part of `__all__`.

I phrased the title of this issue prescriptively, but perhaps it's wrong. We could choose to make `object.attr` completions _intentionally_ looser with respect to how it handles `__all__` than auto-import. We could also make it a setting.

I'm personally inclined to start strict, and then maybe loosen it up in response to user demand.

---

_Label `server` added by @BurntSushi on 2025-12-04 15:42_

---

_Label `completions` added by @BurntSushi on 2025-12-04 15:42_

---
