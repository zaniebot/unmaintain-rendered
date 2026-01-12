```yaml
number: 2211
title: "Type narrowing not applied to `str` after equality check with string literal"
type: issue
state: closed
author: D-Joey-G
labels: []
assignees: []
created_at: 2025-12-24T21:02:25Z
updated_at: 2025-12-24T21:12:46Z
url: https://github.com/astral-sh/ty/issues/2211
synced_at: 2026-01-12T15:54:26Z
```

# Type narrowing not applied to `str` after equality check with string literal

---

_@D-Joey-G_

### Summary

Comparing a `str` with `==` to a Literal does not narrow the type of the str to the corresponding `Literal` type 

[Playground example](https://play.ty.dev/5f0318ce-767f-4232-af94-605c227263f1)

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-12-24 21:12_

Thanks, please see #1566

---

_Closed by @AlexWaygood on 2025-12-24 21:12_

---
