---
number: 14807
title: ruff check -w stuck in file change detected loop when pyproject.toml present (since 0.7.3)
type: issue
state: closed
author: ziddey
labels:
  - bug
  - cli
assignees: []
created_at: 2024-12-06T07:23:07Z
updated_at: 2024-12-06T08:50:30Z
url: https://github.com/astral-sh/ruff/issues/14807
synced_at: 2026-01-10T01:22:55Z
---

# ruff check -w stuck in file change detected loop when pyproject.toml present (since 0.7.3)

---

_Issue opened by @ziddey on 2024-12-06 07:23_

To reproduce (ruff 0.7.3-0.8.2):
```
mkdir test
cd test
touch pyproject.toml
ruff check -w
```

Will be stuck in a `File change detected...` loop.

`ruff.toml` file has the same effect. I've tried using the equivalent default configuration in the docs instead of a blank file with the same result.

0.7.2 seems to work fine with the watch flag

Ubuntu 22.04 LTS amd64 / Debian 12 arm64

---

_Comment by @MichaReiser on 2024-12-06 08:19_

This is interesting. It seems that ruff is constantly notified about changed

---

_Label `bug` added by @MichaReiser on 2024-12-06 08:30_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-12-06 08:30_

---

_Referenced in [astral-sh/ruff#14809](../../astral-sh/ruff/pulls/14809.md) on 2024-12-06 08:34_

---

_Label `cli` added by @MichaReiser on 2024-12-06 08:38_

---

_Closed by @MichaReiser on 2024-12-06 08:50_

---

_Closed by @MichaReiser on 2024-12-06 08:50_

---
