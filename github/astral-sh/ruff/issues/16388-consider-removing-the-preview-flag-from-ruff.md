---
number: 16388
title: "Consider removing the `--preview` flag from `ruff server`"
type: issue
state: open
author: dhruvmanila
labels:
  - server
assignees: []
created_at: 2025-02-26T05:13:40Z
updated_at: 2025-02-26T05:13:40Z
url: https://github.com/astral-sh/ruff/issues/16388
synced_at: 2026-01-07T13:12:16-06:00
---

# Consider removing the `--preview` flag from `ruff server`

---

_Issue opened by @dhruvmanila on 2025-02-26 05:13_

Currently, the behavior of `ruff server --preview` is to turn on preview mode for both the linter and the formatter.

The original idea with `--preview` was to have server specific preview features in the future along with linter and formatter preview features. But, I think it's much better to use the server setting for this instead.

It's also hard to figure out how to pass `--preview` when starting the server within editor settings. Mostly, it involves overriding the command that's used to start the server within the editor setting but users would need to figure that out.

This is a low priority issue. I just wanted to put it down before it slips my mind.

---

_Label `server` added by @dhruvmanila on 2025-02-26 05:13_

---

_Referenced in [astral-sh/ruff#16610](../../astral-sh/ruff/issues/16610.md) on 2025-03-11 06:27_

---
