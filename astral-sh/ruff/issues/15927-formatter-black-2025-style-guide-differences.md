```yaml
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
synced_at: 2026-01-12T15:54:55Z
```

# [formatter]: Black 2025 style guide differences

---

_@MichaReiser_

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

_Comment by @laymonage on 2025-09-17 09:27_

Is now the right time to revisit this? Really looking forward to https://github.com/astral-sh/ruff/issues/9745 😄 

---

_Comment by @MichaReiser on 2025-09-17 09:29_

@dylwil3 and I just chatted yesterday that we should start outlining our plans for the 2026 style guide. So yes, now is the time ;)

---
