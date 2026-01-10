---
number: 5967
title: "`uv lock` could display latest-available versions"
type: issue
state: open
author: charliermarsh
labels:
  - cli
assignees: []
created_at: 2024-08-09T17:19:05Z
updated_at: 2024-08-20T18:21:37Z
url: https://github.com/astral-sh/uv/issues/5967
synced_at: 2026-01-10T01:23:54Z
---

# `uv lock` could display latest-available versions

---

_Issue opened by @charliermarsh on 2024-08-09 17:19_

This is nice:

```
uv on  charlie/update:main [$✘⇡] is 📦 v0.2.34 via 🐍 v3.12.3 via 🦀 v1.80.0
❯ cargo update
    Updating git repository `https://github.com/astral-sh/reqwest-middleware`
    Updating crates.io index
    Updating git repository `https://github.com/charliermarsh/rs-async-zip`
    Updating git repository `https://github.com/astral-sh/pubgrub`
     Locking 551 packages to latest compatible versions
      Adding addr2line v0.22.0 (latest: v0.24.1)
      Adding ahash v0.7.8 (latest: v0.8.11)
      Adding anes v0.1.6 (latest: v0.2.0)
      Adding base64 v0.21.7 (latest: v0.22.1)
      Adding bitflags v1.3.2 (latest: v2.6.0)
      Adding bytecheck v0.6.12 (latest: v0.7.0)
      Adding bytecheck_derive v0.6.12 (latest: v0.7.0)
      Adding cfg_aliases v0.1.1 (latest: v0.2.1)
      Adding data-url v0.2.0 (latest: v0.3.1)
      Adding deadpool v0.10.0 (latest: v0.12.1)
      Adding encode_unicode v0.3.6 (latest: v1.0.0)
      Adding fixedbitset v0.4.2 (latest: v0.5.7)
      Adding fontdb v0.12.0 (latest: v0.21.0)
      Adding generic-array v0.14.7 (latest: v1.1.0)
      Adding gif v0.12.0 (latest: v0.13.1)
      Adding gimli v0.29.0 (latest: v0.31.0)
      Adding hashbrown v0.12.3 (latest: v0.14.5)
      Adding heck v0.4.1 (latest: v0.5.0)
```


---

_Label `cli` added by @charliermarsh on 2024-08-09 17:19_

---

_Label `preview` added by @charliermarsh on 2024-08-09 17:19_

---

_Comment by @zanieb on 2024-08-09 17:22_

Fancy! Is this `uv lock --upgrade`?

---

_Comment by @charliermarsh on 2024-08-09 17:23_

I think it would just be part of `uv lock`

---

_Renamed from "`uv update` could display latest-available versions" to "`uv lock` could display latest-available versions" by @charliermarsh on 2024-08-10 18:55_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---
