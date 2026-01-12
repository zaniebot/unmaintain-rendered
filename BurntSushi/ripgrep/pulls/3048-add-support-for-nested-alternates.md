```yaml
number: 3048
title: Add support for nested alternates
type: pull_request
state: closed
author: lukesandberg
labels:
  - rollup
assignees: []
base: master
head: serialize_globset
created_at: 2025-05-12T22:01:27Z
updated_at: 2025-09-20T01:08:36Z
url: https://github.com/BurntSushi/ripgrep/pull/3048
synced_at: 2026-01-12T18:23:15Z
```

# Add support for nested alternates

---

_@lukesandberg_

This adds support for nesting alternates.

e.g. `**/{node_modules/**/*/{ts,js},crates/**/*.{rs,toml}}` would select all ts/js files found under `node_modules` and all `rs/toml` files found under `crates` in a single glob pattern.


---

_Label `rollup` added by @BurntSushi on 2025-08-17 13:22_

---

_@BurntSushi approved on 2025-08-17 13:26_

Nice, thank you!!!

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
