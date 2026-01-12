```yaml
number: 276
title: Show user configuration file path in --help
type: pull_request
state: closed
author: andersk
labels: []
assignees: []
base: main
head: help-config-path
created_at: 2022-09-28T22:06:27Z
updated_at: 2022-09-29T02:49:48Z
url: https://github.com/astral-sh/ruff/pull/276
synced_at: 2026-01-12T15:55:04Z
```

# Show user configuration file path in --help

---

_@andersk_

Refs https://github.com/charliermarsh/ruff/pull/215#discussion_r973620904, #248.

Note that the `after_help()` computation runs on every invocation of `ruff`, not just `ruff --help`, which is less than ideal but probably fine. I’ll let you decide whether it’s worthwhile.

---

_Comment by @charliermarsh on 2022-09-29 02:17_

I think I may prefer what we have with `--show_settings`?

---

_Comment by @andersk on 2022-09-29 02:49_

What that doesn’t do is tell you where you’d put a user config file if you don’t already have one.

Maybe it could be improved to do that, or maybe it doesn’t matter much.

---

_Closed by @andersk on 2022-09-29 02:49_

---
