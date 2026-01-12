```yaml
number: 776
title: Propagate exit code through Python __main__ wrapper
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: wrapper-exit
created_at: 2022-11-16T19:03:40Z
updated_at: 2022-11-20T01:59:36Z
url: https://github.com/astral-sh/ruff/pull/776
synced_at: 2026-01-12T05:48:45Z
```

# Propagate exit code through Python __main__ wrapper

---

_Pull request opened by @andersk on 2022-11-16 19:03_

Fixes #775.

---

_Review comment by @Jackenmen on `ruff/__main__.py`:7 on 2022-11-16 19:30_

`exit()` is not a built-in function and is only defined if the site module is imported during Python's initialization (which, for example, can be disabled with `-S` option passed to the interpreter). You should use `sys.exit()` instead:
```suggestion
    sys.exit(os.spawnv(os.P_WAIT, ruff, [ruff, *sys.argv[1:]]))
```

---

_@Jackenmen reviewed on 2022-11-16 19:30_

---

_@andersk reviewed on 2022-11-16 20:26_

---

_Review comment by @andersk on `ruff/__main__.py`:7 on 2022-11-16 20:26_

Fixed, thanks. (Ruff should have a rule for this!)

---

_Merged by @charliermarsh on 2022-11-16 21:16_

---

_Closed by @charliermarsh on 2022-11-16 21:16_

---

_Branch deleted on 2022-11-20 01:59_

---
