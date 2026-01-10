```yaml
number: 21055
title: "Conflicting rules `RET505` and `TRY300` when if block is wrapped in try block"
type: issue
state: closed
author: IBuckton
labels: []
assignees: []
created_at: 2025-10-24T05:26:17Z
updated_at: 2025-10-24T05:28:11Z
url: https://github.com/astral-sh/ruff/issues/21055
synced_at: 2026-01-10T11:10:00Z
```

# Conflicting rules `RET505` and `TRY300` when if block is wrapped in try block

---

_Issue opened by @IBuckton on 2025-10-24 05:26_

### Summary

### Summary
Extends upon https://github.com/astral-sh/ruff/issues/17264

```
    try:
        if isinstance(val, dict):
            return val
        return {"data": val}
    except Exception as e:
        print(e)
```

Consider moving this statement to an else block- [TRY300](https://docs.astral.sh/ruff/rules/try-consider-else)

If the rule is followed then RET505 gets triggered

```
    try:
        if isinstance(val, dict):
            return val
        else:
            return {"data": val}
    except Exception as e:
        print(e)
```

Unnecessary else after return statement - [RET505](https://docs.astral.sh/ruff/rules/superfluous-else-return)

[Playgound example](https://play.ruff.rs/6bc0bcf9-5e8a-43e1-a0f7-191bab625512)


---

_Comment by @IBuckton on 2025-10-24 05:28_

Solution is to move else below except
```
    try:
        if isinstance(val, dict):
            return val
    except Exception as e:
        print(e)
    else:
        return {"data": val}
```

---

_Closed by @IBuckton on 2025-10-24 05:28_

---
