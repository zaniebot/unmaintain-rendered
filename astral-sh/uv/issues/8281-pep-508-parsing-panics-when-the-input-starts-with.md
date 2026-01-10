---
number: 8281
title: PEP 508 parsing panics when the input starts with whitespace
type: issue
state: closed
author: InSyncWithFoo
labels: []
assignees: []
created_at: 2024-10-17T06:33:37Z
updated_at: 2024-10-17T12:23:00Z
url: https://github.com/astral-sh/uv/issues/8281
synced_at: 2026-01-10T01:24:26Z
---

# PEP 508 parsing panics when the input starts with whitespace

---

_Issue opened by @InSyncWithFoo on 2024-10-17 06:33_

Minimal reproducible example:

```shell
$ uv init .
$ uv add " ruff"
```

```text
thread 'main' panicked at crates\uv-pep508\src\cursor.rs:40:20:
byte index 6 is out of bounds of ` ruff`
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

[Originally reported](https://discord.com/channels/1039017663004942429/1207998321562619954/1296340012001923083) by dice on Discord.

---

_Referenced in [astral-sh/uv#8282](../../astral-sh/uv/pulls/8282.md) on 2024-10-17 07:02_

---

_Closed by @charliermarsh on 2024-10-17 12:23_

---

_Closed by @charliermarsh on 2024-10-17 12:23_

---
