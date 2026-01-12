```yaml
number: 6771
title: Include trailing end-of-line comments in the measured line width
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-08-22T14:32:03Z
updated_at: 2023-08-30T08:07:13Z
url: https://github.com/astral-sh/ruff/issues/6771
synced_at: 2026-01-12T15:54:46Z
```

# Include trailing end-of-line comments in the measured line width

---

_@MichaReiser_

_No description provided._

---

_Label `formatter` added by @MichaReiser on 2023-08-22 14:32_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:32_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-08-24 08:41_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-24 08:41_

---

_Comment by @cnpryer on 2023-08-26 02:05_

I can help look into this, but how is it different from #5630? Are there ever situations where a trailing end-of-line comment isn't handled as a line suffix?

---

_Comment by @MichaReiser on 2023-08-26 06:39_

> I can help look into this, but how is it different from #5630? Are there ever situations where a trailing end-of-line comment isn't handled as a line suffix?

#5630 is only about adding the reserved_width field to LineSuffix but without changing the formatting yet (always initialise it with 0). This issue is about changing the default to the actual comment with. 

---

_Assigned to @cnpryer by @MichaReiser on 2023-08-28 08:26_

---

_Closed by @MichaReiser on 2023-08-30 08:07_

---
