```yaml
number: 188
title: "Consider making `source_text` return a `Result`"
type: issue
state: closed
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2025-02-26T17:07:28Z
updated_at: 2025-07-09T10:38:05Z
url: https://github.com/astral-sh/ty/issues/188
synced_at: 2026-01-10T02:07:35Z
```

# Consider making `source_text` return a `Result`

---

_Issue opened by @MichaReiser on 2025-02-26 17:07_

We may want to LRU source texts in the future. This introduces the risk that re-reading the source text after the cached text was claimed may fail, in which case the text ranges may no longer be trusted. 

This is only a problem if we start evicting LRU cached results in the middle of a revision. 

---

_Renamed from "[red-knot] Consider making `source_text` return a `Result`" to "Consider making `source_text` return a `Result`" by @MichaReiser on 2025-05-07 15:26_

---

_Label `internal` added by @AlexWaygood on 2025-05-11 07:28_

---

_Comment by @MichaReiser on 2025-07-09 10:38_

source text isn't as dominant in our memory profiles and enabling LRU might not give us much. I'll close this for now

---

_Closed by @MichaReiser on 2025-07-09 10:38_

---
