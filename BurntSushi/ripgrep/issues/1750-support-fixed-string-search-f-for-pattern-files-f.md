```yaml
number: 1750
title: Support fixed string search (-F) for pattern files (-f)
type: issue
state: closed
author: jonashaag
labels:
  - invalid
assignees: []
created_at: 2020-11-29T16:52:31Z
updated_at: 2020-11-29T16:54:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1750
synced_at: 2026-01-12T16:13:24Z
```

# Support fixed string search (-F) for pattern files (-f)

---

_@jonashaag_

#### Describe your feature request

I want to search for lots of patterns from a pattern file in another file with lots of lines. Here, lots = 50k.

So, what I want is a faster version of `grep -f needles haystack`.

I think currently `-F` is ignored when using `-f`, causing ripgrep to crash because it tries to create a giant regex from 50k patterns.

---

_Comment by @BurntSushi on 2020-11-29 16:53_

> causing ripgrep to crash

This sounds like a bug report to me. Please file a new issue and fill out the bug report template.

---

_Closed by @BurntSushi on 2020-11-29 16:53_

---

_Label `invalid` added by @BurntSushi on 2020-11-29 16:54_

---
