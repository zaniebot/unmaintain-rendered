```yaml
number: 366
title: "Don't persist no-cache cache dir "
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-08T16:21:45Z
updated_at: 2023-11-16T20:53:11Z
url: https://github.com/astral-sh/uv/issues/366
synced_at: 2026-01-12T15:58:23Z
```

# Don't persist no-cache cache dir 

---

_@konstin_

`into_path()` persists the cache dir, which we want to avoid.

https://github.com/astral-sh/puffin/blob/d407bbbee6bc6553d7eff1eda5324919fb8dd1f2/crates/puffin-cli/src/main.rs#L203-L217

The ideal solution would be `Cache` abstraction that internally carries an optional (e.g. an enum variant) `TempDir`, derefs to `Path` and has a reusable constructor that takes `no_cache` and `cache_dir` arguments.

---

_Comment by @charliermarsh on 2023-11-08 16:24_

We do this in a few other places too.

---

_Label `bug` added by @charliermarsh on 2023-11-09 01:34_

---

_Added to milestone `Future` by @charliermarsh on 2023-11-09 01:34_

---

_Comment by @charliermarsh on 2023-11-16 20:53_

Closed by https://github.com/astral-sh/puffin/pull/437.

---

_Closed by @charliermarsh on 2023-11-16 20:53_

---
