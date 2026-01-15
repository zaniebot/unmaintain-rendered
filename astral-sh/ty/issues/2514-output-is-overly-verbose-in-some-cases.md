```yaml
number: 2514
title: output is overly verbose in some cases
type: issue
state: open
author: danielgafni
labels:
  - bug
  - help wanted
  - diagnostics
assignees: []
created_at: 2026-01-15T12:27:14Z
updated_at: 2026-01-15T12:36:59Z
url: https://github.com/astral-sh/ty/issues/2514
synced_at: 2026-01-15T12:52:53Z
```

# output is overly verbose in some cases

---

_@danielgafni_

### Summary

`ty check` prints the entire source module and generates ~3k lines of output for this example (install `narwhals` first): 

```py
from narwhals.typing import FrameT

def fn(
    frame: FrameT
) -> FrameT:
    return frame.with_columns(
        nw.lit("hello world").alias("foo")
    )
```

[output.txt](https://github.com/user-attachments/files/24641469/output.txt)

### Version

0.0.12

---

_Comment by @danielgafni on 2026-01-15 12:28_

@MichaReiser made this issue as requested 

---

_Comment by @AlexWaygood on 2026-01-15 12:29_

Wow, hmm, yes, seems a bit unnecessary :-)

Thanks for the report, definitely a bug 😆

---

_Label `bug` added by @AlexWaygood on 2026-01-15 12:29_

---

_Label `diagnostics` added by @AlexWaygood on 2026-01-15 12:29_

---

_Comment by @MichaReiser on 2026-01-15 12:32_

Haha, this is great. We should probably only highlight the class header

---

_Label `help wanted` added by @MichaReiser on 2026-01-15 12:32_

---

_Comment by @danielgafni on 2026-01-15 12:36_

I bet LLM providers love this bug :) 

---
