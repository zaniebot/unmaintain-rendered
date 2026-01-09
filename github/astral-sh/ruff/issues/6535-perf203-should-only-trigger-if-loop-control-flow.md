---
number: 6535
title: "PERF203 should only trigger if loop control flow isn't in the try block"
type: issue
state: closed
author: evanrittenhouse
labels: []
assignees: []
created_at: 2023-08-13T14:29:18Z
updated_at: 2024-09-16T01:22:08Z
url: https://github.com/astral-sh/ruff/issues/6535
synced_at: 2026-01-07T13:12:15-06:00
---

# PERF203 should only trigger if loop control flow isn't in the try block

---

_Issue opened by @evanrittenhouse on 2023-08-13 14:29_

Copied from https://github.com/astral-sh/ruff/issues/4789#issuecomment-1635619940:

PERF203 should only trigger when there’s no loop flow control (break, continue) in the try-catch statement. E.g. I don’t think there’s a better way to write the following:

```
for repo in ['R-patched', 'cran', 'bioc']:
    try:
        pkg_cache = self._fetch_cache(repo, pkg)
        break
    except HTTPError:
        pass
else:
    return None
```

---

_Referenced in [astral-sh/ruff#6536](../../astral-sh/ruff/pulls/6536.md) on 2023-08-13 14:29_

---

_Closed by @charliermarsh on 2023-08-14 20:47_

---

_Referenced in [astral-sh/ruff#11602](../../astral-sh/ruff/issues/11602.md) on 2024-05-29 17:41_

---

_Comment by @trim21 on 2024-09-16 01:16_

I just find that this false positive still exists, but only on certain target-version.

it pass with `target-version = 'py311'` but failed with `target-version = 'py38'`


https://play.ruff.rs/eadb6e43-5f78-4bcd-93f3-2d409bdbf4c8

---
