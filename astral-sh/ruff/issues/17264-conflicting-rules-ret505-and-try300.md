```yaml
number: 17264
title: "Conflicting rules `RET505` and `TRY300`"
type: issue
state: closed
author: AITechXcel
labels:
  - needs-mre
assignees: []
created_at: 2025-04-07T04:52:52Z
updated_at: 2025-04-28T08:35:08Z
url: https://github.com/astral-sh/ruff/issues/17264
synced_at: 2026-01-12T15:54:55Z
```

# Conflicting rules `RET505` and `TRY300`

---

_@AITechXcel_

### Summary

```python
                    if isinstance(val, dict):
                        return val
                    else:
                        return {"data": val}
```

`Unnecessary else after return statement`

Ruff[RET505](https://docs.astral.sh/ruff/rules/superfluous-else-return)

And if I do that:

```python
                    if isinstance(val, dict):
                        return val
                    return {"data": val}
```

`Consider moving this statement to an `else` block`

Ruff[TRY300](https://docs.astral.sh/ruff/rules/try-consider-else)

It needs to make up its mind. Which should it be?

### Version

    "ruff==0.9.8",

---

_Renamed from "Conflicting rules" to "Conflicting rules `RET505` and `TRY300`" by @MichaReiser on 2025-04-07 06:50_

---

_Label `needs-mre` added by @MichaReiser on 2025-04-07 06:53_

---

_Comment by @MichaReiser on 2025-04-07 06:53_

Hi @AITechXcel 

I'm unable to reproduce the conflict, see [playground](https://play.ruff.rs/bcd7c31b-8589-472a-a48c-d73aa50d07dc)

For `Try300` to raise, it requires at least a try-catch statement. Could you share a complete example including the try except so that we can reproduce the incompatibility

---

_Closed by @MichaReiser on 2025-04-28 08:35_

---
