---
number: 5192
title: Bogus resolution when index is prefilled from lockfile
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-18T16:41:08Z
updated_at: 2024-07-18T17:02:09Z
url: https://github.com/astral-sh/uv/issues/5192
synced_at: 2026-01-07T13:12:17-06:00
---

# Bogus resolution when index is prefilled from lockfile

---

_Issue opened by @konstin on 2024-07-18 16:41_

* Take https://github.com/konstin/packse/blob/1f70cbe8ddd572eb93e0a3679ce96a85c5017c78/scenarios/fork/preferences-dependent-forking.toml
* Run the packse server
* Create a project:
```
[project]
name = "dummy"
version = "4.39.0.dev0"
requires-python = ">=3.12"
dependencies = [
  "preferences-dependent-forking-bb915163",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Run the following commands
```
cargo run sync --index-url http://127.0.0.1:3141 # Initialize uv.lock
cargo run sync --index-url http://127.0.0.1:3141 # <- This one is bogus
```

Due to https://github.com/astral-sh/uv/blob/6a49dba30cf618bf6e61088b3655ffd8a10d8dc3/crates/uv-resolver/src/lock.rs#L2071, we will install `preferences-dependent-forking-foo-bb915163==2.0.0` on linux, even though `preferences-dependent-forking-bar-bb915163==1.0.0` requires `preferences-dependent-forking-foo-bb915163==1.0.0` on linux.


---

_Label `bug` added by @konstin on 2024-07-18 16:41_

---

_Label `preview` added by @konstin on 2024-07-18 16:41_

---

_Assigned to @ibraheemdev by @ibraheemdev on 2024-07-18 16:43_

---

_Referenced in [astral-sh/uv#5193](../../astral-sh/uv/pulls/5193.md) on 2024-07-18 16:48_

---

_Closed by @konstin on 2024-07-18 17:02_

---
