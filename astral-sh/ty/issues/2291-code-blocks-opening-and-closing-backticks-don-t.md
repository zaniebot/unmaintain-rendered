```yaml
number: 2291
title: "Code blocks opening and closing backticks don't match"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-31T08:22:27Z
updated_at: 2026-01-05T12:13:26Z
url: https://github.com/astral-sh/ty/issues/2291
synced_at: 2026-01-12T15:54:26Z
```

# Code blocks opening and closing backticks don't match

---

_@MichaReiser_

### Summary

Hover `EncoderDecoderCache` in https://play.ty.dev/625b8706-3dac-457b-8909-51c78e5d2667 and note how the code block has many opening backticks but only 3 closing backticks

### Version

_No response_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 08:22_

---

_Label `server` added by @MichaReiser on 2025-12-31 08:22_

---

_Comment by @MichaReiser on 2025-12-31 08:28_

@Gankra, here's an example where our markdown parsing seems to break down

---

_Assigned to @Gankra by @MichaReiser on 2025-12-31 08:28_

---

_Label `bug` added by @MichaReiser on 2025-12-31 08:28_

---

_Comment by @Gankra on 2026-01-04 18:58_

Ah this is "cute" -- I believe we don't actually try to parse markdown code fences, because leaving them verbatim is the desire... but that also means we don't disable any of our parsing and so we immediately see the PS1 prompts and make our own code fence inside the code fence and mess up the real code fence!

---

_Comment by @Gankra on 2026-01-04 19:00_

We end up generating:

       ```python
       `````````python
       >>> blahblah
       ```
       `````````

---

_Comment by @Gankra on 2026-01-04 19:09_

This is possibly the first serious instance of "of course you're parsing reST-markdown soup, just handle both at once!" (a quick skim doesn't reveal any reference to markdown codefences being valid reST/sphinx syntax, but I'm not sure that means much).

---

_Closed by @Gankra on 2026-01-05 12:13_

---
