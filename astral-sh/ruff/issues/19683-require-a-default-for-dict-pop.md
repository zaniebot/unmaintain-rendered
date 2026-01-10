```yaml
number: 19683
title: Require a default for dict.pop
type: issue
state: open
author: ashrub-holvi
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-08-01T09:03:49Z
updated_at: 2025-08-01T13:13:43Z
url: https://github.com/astral-sh/ruff/issues/19683
synced_at: 2026-01-10T11:09:59Z
```

# Require a default for dict.pop

---

_Issue opened by @ashrub-holvi on 2025-08-01 09:03_

### Summary

Hi,

I know you are busy with bigger stuff, but documenting it for future, don't see similar issues.

[dict.pop](https://docs.python.org/3/library/stdtypes.html#dict.pop) has an optional default argument, but it's behaviour is different from [dict.get](https://docs.python.org/3/library/stdtypes.html#dict.get) - `pop` may raise a KeyError, but `get` never do it and IMO it's easy to forget how `pop` is working because `get` is used much more often.
Perhaps good to have a rule which requires to set a default, maybe if code is not handling KeyError, but not sure later is possible.

---

_Comment by @ntBre on 2025-08-01 13:13_

Thanks for the suggestion!

---

_Label `rule` added by @ntBre on 2025-08-01 13:13_

---

_Label `needs-decision` added by @ntBre on 2025-08-01 13:13_

---
