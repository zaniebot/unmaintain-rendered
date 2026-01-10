```yaml
number: 1805
title: Fix fastText installation bug
type: pull_request
state: closed
author: yasufumy
labels: []
assignees: []
draft: true
base: main
head: fix/fasttext-installation
created_at: 2024-02-21T13:15:41Z
updated_at: 2024-02-21T13:28:40Z
url: https://github.com/astral-sh/uv/pull/1805
synced_at: 2026-01-10T15:33:24Z
```

# Fix fastText installation bug

---

_Pull request opened by @yasufumy on 2024-02-21 13:15_

## Summary

Resolve #1446

I added a new build backend for fastText to avoid the installation error. I'm not sure if this is the right way to handle the issue, so let me know if I'm doing it wrong.


---

_Comment by @charliermarsh on 2024-02-21 13:26_

Unfortunately I think we're unlikely to accept this -- it's been fixed on the `fasttext` side, IIUC, but if we add a special-case like this for a specific package, we have to maintain it forever.

---

_Closed by @yasufumy on 2024-02-21 13:28_

---

_Branch deleted on 2024-02-21 13:28_

---
