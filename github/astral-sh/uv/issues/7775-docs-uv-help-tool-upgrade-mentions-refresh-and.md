---
number: 7775
title: "[docs] `uv help tool upgrade` mentions `--refresh` and `--refresh-package` options that do not exist for it"
type: issue
state: open
author: ilyagr
labels:
  - cli
assignees: []
created_at: 2024-09-29T05:34:30Z
updated_at: 2024-10-16T20:11:56Z
url: https://github.com/astral-sh/uv/issues/7775
synced_at: 2026-01-07T13:12:17-06:00
---

# [docs] `uv help tool upgrade` mentions `--refresh` and `--refresh-package` options that do not exist for it

---

_Issue opened by @ilyagr on 2024-09-29 05:34_

I am using `uv 0.4.17 (d2e7b40ce 2024-09-27)`.

The docs for `uv tool upgrade` are a bit confusing. The description of the `-U` option mentions the `--refresh` option that does not exist for this command. The description of `-P` option mentions a similarly nonexistent `--refresh-package`.

Moreover, `-U` is turned on by default, which is unclear from the help (but there's a message explaining it when you try to use `uv tool upgrade -U`).

-----

The reason I got caught up in this, by the way, is that I wanted an option that does not update the tool's dependencies unless the tool can be updated as well. See #7776 .

---

_Comment by @zanieb on 2024-09-29 15:45_

Thanks!

These options come from a shared struct which contains all options for commands that resolve and install. We should probably override and omit them though, as I don't think `--upgrade` and `--upgrade-package` don't make sense here?

---

_Label `cli` added by @zanieb on 2024-09-29 15:45_

---

_Comment by @callegar on 2024-10-16 20:11_

You see the explanations for those options by asking for the help for `uv tool install` rather than `upgrade`. Maybe, just adding a line "see also the help for `uv tool install`" could be enough.

---
