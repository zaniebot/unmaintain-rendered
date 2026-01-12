```yaml
number: 537
title: "Compress the source text stored in `SourceText`"
type: issue
state: closed
author: MichaReiser
labels:
  - memory
assignees: []
created_at: 2025-05-28T12:11:53Z
updated_at: 2025-08-15T07:01:46Z
url: https://github.com/astral-sh/ty/issues/537
synced_at: 2026-01-12T15:54:23Z
```

# Compress the source text stored in `SourceText`

---

_@MichaReiser_

ty holds on to all read source files because it can't purge the file without risking that it won't be able to re-read the file later. 

We should explore the possibility of storing the compressed source text instead of the raw string. 

---

_Label `memory` added by @MichaReiser on 2025-05-28 12:11_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:47_

---

_Assigned to @ibraheemdev by @MichaReiser on 2025-06-24 10:40_

---

_Comment by @MichaReiser on 2025-06-24 10:41_

@ibraheemdev can you comment here with your findings and either close this issue (if you think it's not worth doing) or leave it open until we have more accurate numbers, thanks to your upstream salsa work.

---

_Comment by @ibraheemdev on 2025-06-24 19:53_

I think we should keep this open until we have better memory usage numbers for salsa values, it's hard to measure if this is worth it without that.

---

_Comment by @MichaReiser on 2025-08-15 07:01_

We now have those numbers and they show that source text is neglectibe in the grand scheme of things (this may change in the future if we make other optimizations but we can then revisit if this is worth exploring)

---

_Closed by @MichaReiser on 2025-08-15 07:01_

---
