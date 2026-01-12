```yaml
number: 7536
title: Unable to change cache directory
type: issue
state: closed
author: ryscu7
labels: []
assignees: []
created_at: 2024-09-19T08:20:33Z
updated_at: 2024-09-19T09:50:32Z
url: https://github.com/astral-sh/uv/issues/7536
synced_at: 2026-01-12T15:59:14Z
```

# Unable to change cache directory

---

_@ryscu7_

Setting a custom cache directory to avoid the hardlink warning doesn't work.

I am on Windows 11
UV version: 0.4.12 (2545bca69 2024-09-18)

```
V:\
❯ uv cache dir --verbose
DEBUG uv 0.4.12
C:\Users\LENOVO\AppData\Local\uv\cache

V:\
❯ uv cache dir --cache-dir "V:\uv\cache" --verbose
DEBUG uv 0.4.12
V:\uv\cache

V:\
❯ uv cache dir --verbose
DEBUG uv 0.4.12
C:\Users\LENOVO\AppData\Local\uv\cache
```

---

_Closed by @ryscu7 on 2024-09-19 09:49_

---

_Comment by @ryscu7 on 2024-09-19 09:50_

I fixed it by adding the new custom cache directory path to the `uv.toml` config file

```toml
cache-dir = "./.uv_cache"
```

---
