---
number: 3421
title: "Implement `flake8-clean-block`"
type: issue
state: open
author: sasanjac
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-03-09T16:11:24Z
updated_at: 2023-07-10T01:26:08Z
url: https://github.com/astral-sh/ruff/issues/3421
synced_at: 2026-01-10T01:22:42Z
---

# Implement `flake8-clean-block`

---

_Issue opened by @sasanjac on 2023-03-09 16:11_

[`flake8-clean-block`](https://github.com/cyyc1/flake8-clean-block) enforces a blank line after indented blocks to increase code readability.

- [ ] `CLB100`: no blank line after the end of an indented block

---

_Comment by @JonathanPlasse on 2023-03-09 18:19_

Maybe it should be a configuration for the Ruff formatter.

---

_Label `plugin` added by @charliermarsh on 2023-03-18 04:09_

---

_Comment by @Avasam on 2023-06-23 01:57_

I like this. It overlaps with a couple pycodestyle `E30` rules too (but without forcing 2 lines, which I dislike and wish I could configure out of autopep8)

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---
