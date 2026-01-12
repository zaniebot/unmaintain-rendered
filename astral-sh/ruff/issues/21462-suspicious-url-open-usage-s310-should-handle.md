```yaml
number: 21462
title: suspicious-url-open-usage (S310) should handle string literal
type: issue
state: closed
author: rajeee
labels:
  - rule
assignees: []
created_at: 2025-11-14T21:31:58Z
updated_at: 2025-11-26T09:21:52Z
url: https://github.com/astral-sh/ruff/issues/21462
synced_at: 2026-01-12T15:54:57Z
```

# suspicious-url-open-usage (S310) should handle string literal

---

_@rajeee_

### Summary

```
import urllib.request
path = "https://example.com/data.csv"
urllib.request.urlretrieve(path, "data.csv")
```
This currently raises suspicious-url-open-usage (S310), but it should not because path can be inferred to have Literal['https://example.com/data.csv'] type and that's safe. 

---

_Renamed from "S310 should handle string literal" to "suspicious-url-open-usage (S310) should handle string literal" by @rajeee on 2025-11-14 21:40_

---

_Comment by @ntBre on 2025-11-14 23:11_

I think it could make sense to try resolving the path argument to a binding to a string literal like we do here:

https://github.com/astral-sh/ruff/blob/f19c21914b11a3ba6755db9cd6449b7ba1212e64/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs#L397-L400

---

_Label `rule` added by @ntBre on 2025-11-14 23:11_

---

_Closed by @MichaReiser on 2025-11-26 09:21_

---
