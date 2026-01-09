---
number: 1064
title: "[feature-request] Allow --watch to automatically fix when combined with --fix"
type: issue
state: closed
author: phillipuniverse
labels:
  - good first issue
  - cli
assignees: []
created_at: 2022-12-05T15:37:27Z
updated_at: 2023-04-20T03:33:14Z
url: https://github.com/astral-sh/ruff/issues/1064
synced_at: 2026-01-07T13:12:14-06:00
---

# [feature-request] Allow --watch to automatically fix when combined with --fix

---

_Issue opened by @phillipuniverse on 2022-12-05 15:37_

Running the following:

```
ruff . --fix --watch
```

I would expect that Ruff would automatically fix files that it watches, not just display the errors. But it appears that the `--watch` clobbers `--fix` in that it will watch files and just report the errors.

If I stop watching and then `ruff . --fix`, ruff automatically fixes issues that it found.

---

_Renamed from "[feature-request] Allow --watch to automatically fix with --fix" to "[feature-request] Allow --watch to automatically fix when combined with --fix" by @phillipuniverse on 2022-12-05 15:38_

---

_Label `enhancement` added by @charliermarsh on 2022-12-05 15:40_

---

_Comment by @charliermarsh on 2022-12-05 18:08_

I initially held off on this because `--watch` is based on file-watching, and autofixing will modify those files thus triggering another invocation. It may actually work as-is, though. I can at least test it out.

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:16_

---

_Label `cli` added by @charliermarsh on 2022-12-31 18:16_

---

_Label `good first issue` added by @charliermarsh on 2022-12-31 18:28_

---

_Referenced in [astral-sh/ruff#4035](../../astral-sh/ruff/pulls/4035.md) on 2023-04-20 01:01_

---

_Closed by @charliermarsh on 2023-04-20 03:33_

---
