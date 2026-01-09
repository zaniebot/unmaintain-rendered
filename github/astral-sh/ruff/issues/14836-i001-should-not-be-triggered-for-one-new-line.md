---
number: 14836
title: "`I001` should not be triggered for one new line after imports"
type: issue
state: closed
author: samuelcolvin
labels:
  - question
  - isort
assignees: []
created_at: 2024-12-08T13:34:35Z
updated_at: 2024-12-12T19:25:24Z
url: https://github.com/astral-sh/ruff/issues/14836
synced_at: 2026-01-07T13:12:16-06:00
---

# `I001` should not be triggered for one new line after imports

---

_Issue opened by @samuelcolvin on 2024-12-08 13:34_

Background: see https://github.com/pydantic/pytest-examples/issues/46, in [pytest-examples](https://github.com/pydantic/pytest-examples) we want to allow one new line between functions to keep examples short.

This works well in general, but breaks when you have a function directly after imports where IMHO ruff `I001` is overreaching in complaining about "un-sorted or un-formatted" because there's one blank line, not two.

(We use black for formatting as it's faster than ruff running as a separate subprocess (side note, would be great if you could release `ruff` as a Python library so we can use it embeded), and ruff for linting.)

I don't think the error `Import block is un-sorted or un-formatted` makes sense for this code:

```py
from typing import Union

from pydantic_ai import RunContext, Tool
from pydantic_ai.tools import ToolDefinition

async def only_if_42(
    ctx: RunContext[int], tool_def: ToolDefinition
) -> Union[ToolDefinition, None]:
    if ctx.deps == 42:
        return tool_def

def hitchhiker(ctx: RunContext[int], answer: str) -> str:
    return f'{ctx.deps} {answer}'

hitchhiker = Tool(hitchhiker, prepare=only_if_42)
```

There should be a new `I003` code for "expected two blank line after imports", which I could then switch off.

---

Related, the font you use in your docs make capital `I` very hard to read:

![Image](https://github.com/user-attachments/assets/47bd8263-ca9b-46bd-9dff-51899dc6fb7b)


---

_Comment by @MichaReiser on 2024-12-08 13:43_

You can use the `isort.lines-after-imports = 1` if you always one exactly one blank line after imports. The default `-1` uses 1 or 2 empty lines, depending on what comes after the import, which is in line with black/ruff (are you using black's preview style that it doesn't insert two empty lines after the import)?

> (We use black for formatting as it's faster than ruff running as a separate subprocess (side note, would be great if you could release ruff as a Python library so we can use it embeded), and ruff for linting.)

See https://github.com/astral-sh/ruff/issues/659



---

_Label `question` added by @MichaReiser on 2024-12-08 13:43_

---

_Label `isort` added by @AlexWaygood on 2024-12-08 14:38_

---

_Comment by @samuelcolvin on 2024-12-08 17:56_

`isort.lines-after-imports` is helpful, thank you.

But on balance I do think this should be a new error code too.

---

_Comment by @MichaReiser on 2024-12-08 18:57_

> But on balance I do think this should be a new error code too.

Possibly, but I'd prefer keeping it as is for now to match the upstream isort. We should revisit (and possibly even remove) isort as a whole.

---

_Closed by @MichaReiser on 2024-12-08 18:57_

---

_Comment by @AlexWaygood on 2024-12-08 19:01_

> I'd prefer keeping it as is for now to match the upstream isort

I'm not disputing that lots of users find it confusing that isort is implemented as a lint check rather than part of the formatter, but FWIW, I don't think the upstream isort tool has any error codes

---

_Comment by @MichaReiser on 2024-12-08 20:07_

> 'm not disputing that lots of users find it confusing that isort is implemented as a lint check rather than part of the formatter

Totally agree and yes, the upstream isort has no error codes. But I understand that the idea is that `ISC001` matches upstream (it's how you enable isort today)

---

_Comment by @samuelcolvin on 2024-12-12 19:25_

FWIW I suspect ruff's usage is much greater than isort's ever was, so the name `isort` is a historical curiosity that means nothing to most users.

I think this should be re-opened.

---
