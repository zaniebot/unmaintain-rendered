---
number: 15927
title: "[formatter]: Black 2025 style guide differences"
type: issue
state: open
author: MichaReiser
labels:
  - formatter
  - style
assignees: []
created_at: 2025-02-04T11:25:21Z
updated_at: 2025-09-17T09:29:59Z
url: https://github.com/astral-sh/ruff/issues/15927
synced_at: 2026-01-07T13:12:16-06:00
---

# [formatter]: Black 2025 style guide differences

---

_Issue opened by @MichaReiser on 2025-02-04 11:25_

### Description

The following style changes are implemented in Black 2025 but not in Ruff:

* [remove parentheses around sole list items](https://github.com/psf/black/pull/4312)
* ~~[Whitespace before # fmt: skip comments is no longer normalized](https://github.com/psf/black/pull/4146)~~ It seems Ruff already supports this :)


We should decide if we want to implement those style changes for the 2026 style guide.

---

_Label `formatter` added by @MichaReiser on 2025-02-04 11:25_

---

_Label `style` added by @MichaReiser on 2025-02-04 11:25_

---

_Referenced in [ansible/vscode-ansible#1812](../../ansible/vscode-ansible/pulls/1812.md) on 2025-02-06 19:36_

---

_Comment by @laymonage on 2025-09-17 09:27_

Is now the right time to revisit this? Really looking forward to https://github.com/astral-sh/ruff/issues/9745 😄 

---

_Comment by @MichaReiser on 2025-09-17 09:29_

@dylwil3 and I just chatted yesterday that we should start outlining our plans for the 2026 style guide. So yes, now is the time ;)

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-20 11:12_

---
