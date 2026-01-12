```yaml
number: 6130
title: (🎁) option to automatically cleanup dangling temporary directories
type: issue
state: open
author: KotlinIsland
labels: []
assignees: []
created_at: 2024-08-16T00:19:13Z
updated_at: 2024-08-16T00:19:13Z
url: https://github.com/astral-sh/uv/issues/6130
synced_at: 2026-01-12T15:59:01Z
```

# (🎁) option to automatically cleanup dangling temporary directories

---

_@KotlinIsland_

```py
uv pip install basedmypy
warning: Ignoring dangling temporary directory: `C:\Users\AMONGUS\projects\test\.venv\Lib\site-packages\~asedpyright-1.15.0.dist-info`
```

i always run into these stupid folders because language servers lock files etc and stuff fails to install. it would be very based if `uv` could clean them up for me

---
