```yaml
number: 17198
title: "How to disable lint fixes in region? Should `# fmt: off` disable lint fixes?"
type: issue
state: closed
author: kaddkaka
labels:
  - question
assignees: []
created_at: 2025-04-04T10:44:44Z
updated_at: 2025-04-05T21:39:28Z
url: https://github.com/astral-sh/ruff/issues/17198
synced_at: 2026-01-10T11:09:58Z
```

# How to disable lint fixes in region? Should `# fmt: off` disable lint fixes?

---

_Issue opened by @kaddkaka on 2025-04-04 10:44_

### Question

Imagine some code aligned at `=` like this:
```py
things["start"]  = START_ADDR
things["group0"] = START_ADDR + GR0_OFFSET
```

some whitespace lint fixes will change this to 
```py
things["start"] = START_ADDR
things["group0"] = START_ADDR + GR0_OFFSET
```

Is there some way to disable this happening for this specific region of code? Something like `# fmt: off`?

I couldn't find anything at https://docs.astral.sh/ruff/linter/

### Version

_No response_

---

_Label `question` added by @kaddkaka on 2025-04-04 10:44_

---

_Comment by @MichaReiser on 2025-04-04 11:34_

This is currently not supported, see https://github.com/astral-sh/ruff/issues/3711

You can only disable a lint for a specific line or the entire file. 

I'd recommend you to disable the formatting related lint rules if you're using the formatter. You can then use `fmt:off` and `fmt: on`

---

_Comment by @kaddkaka on 2025-04-05 21:30_

I see, thank you for quick reply. We are currently not using the ruff formatter, so unfortunately that's not an easy out for us. I guess, I'm just condemned to letting the tool mangle this code for now.

(I wish the formatter had a much less aggressive mode that for example didn't insert/joined lines)

---

_Comment by @kaddkaka on 2025-04-05 21:31_

If you believe #3711 fully covers this issue, then this one can be closed. 

---

_Closed by @MichaReiser on 2025-04-05 21:39_

---
