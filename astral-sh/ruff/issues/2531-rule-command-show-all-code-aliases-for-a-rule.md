---
number: 2531
title: "`rule` command: show all code aliases for a rule"
type: issue
state: open
author: ngnpope
labels:
  - cli
assignees: []
created_at: 2023-02-03T12:08:45Z
updated_at: 2023-02-03T13:04:41Z
url: https://github.com/astral-sh/ruff/issues/2531
synced_at: 2026-01-10T01:22:41Z
---

# `rule` command: show all code aliases for a rule

---

_Issue opened by @ngnpope on 2023-02-03 12:08_

Following on from #2186 and #2517, it would be nice to show all codes that match a rule.

Before Example:

```
❯ ruff rule UP004
useless-object-inheritance

Code: UP004 (pyupgrade)

Autofix is always available.

Message formats:

* Class `{name}` inherits from `object`
```

After Example:

```
❯ ruff rule UP004
useless-object-inheritance

Code: PIE792 (flake8-pie), PLR0205 (pylint), UP004 (pyupgrade)

Autofix is always available.

Message formats:

* Class `{name}` inherits from `object`
```

---

_Label `cli` added by @charliermarsh on 2023-02-03 13:04_

---
