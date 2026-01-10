---
number: 10597
title: "uv sync ignores non-existent extras given to `--extra` option"
type: issue
state: closed
author: danielhollas
labels:
  - needs-decision
  - cli
  - breaking
assignees: []
created_at: 2025-01-14T14:15:50Z
updated_at: 2025-06-13T15:15:26Z
url: https://github.com/astral-sh/uv/issues/10597
synced_at: 2026-01-10T01:24:55Z
---

# uv sync ignores non-existent extras given to `--extra` option

---

_Issue opened by @danielhollas on 2025-01-14 14:15_

I've made a typo in the name of the optional dependencies (extras) and was surprised that uv did not throw any error and simply seemed to ignore this.

```
❯ uv --version
uv 0.5.18
❯ uv sync --extra non-existent
Resolved 262 packages in 1ms
Audited 72 packages in 1ms
```

---

_Label `needs-decision` added by @charliermarsh on 2025-01-16 18:15_

---

_Label `cli` added by @zanieb on 2025-01-16 19:06_

---

_Label `breaking` added by @zanieb on 2025-01-16 19:06_

---

_Comment by @zanieb on 2025-01-16 19:06_

I'm in favor.

---

_Added to milestone `v0.6.0` by @zanieb on 2025-01-16 19:06_

---

_Referenced in [astral-sh/uv#10869](../../astral-sh/uv/pulls/10869.md) on 2025-01-22 18:14_

---

_Referenced in [astral-sh/uv#11426](../../astral-sh/uv/pulls/11426.md) on 2025-02-11 20:51_

---

_Comment by @zanieb on 2025-02-13 17:05_

Implemented in https://github.com/astral-sh/uv/pull/11426

---

_Closed by @zanieb on 2025-02-13 17:05_

---

_Comment by @elephaint on 2025-06-13 14:37_

Can I somehow get the old behavior? I want to provide non-existing extras without receiving an error.... (because sometimes the extra exists, sometimes it doesn't). Or how can I beforehand check that the provided extra exists in the pyproject.toml?

---

_Comment by @danielhollas on 2025-06-13 15:15_

Obligatory XKCD :-D

https://xkcd.com/1172/

---
