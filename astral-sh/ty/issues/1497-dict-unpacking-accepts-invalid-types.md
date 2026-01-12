```yaml
number: 1497
title: Dict unpacking accepts invalid types
type: issue
state: closed
author: refi64
labels: []
assignees: []
created_at: 2025-11-06T23:09:33Z
updated_at: 2025-11-07T04:11:38Z
url: https://github.com/astral-sh/ty/issues/1497
synced_at: 2026-01-12T15:54:25Z
```

# Dict unpacking accepts invalid types

---

_@refi64_

### Summary

https://play.ty.dev/6fed81b8-6589-4e8b-9adf-71bc1676d9cc

```python
x = {**1}
```

This succeeds without any errors, even though it of course fails at runtime. This seems to work just fine in list literals (show in the playground link) but not in dicts. Might be related to https://github.com/astral-sh/ty/issues/1332? But that seems to be more specifically about "mappings with the wrong types" rather than "not a mapping at all".

### Version

35640dd85

---

_Closed by @AlexWaygood on 2025-11-07 01:58_

---

_Comment by @AlexWaygood on 2025-11-07 02:13_

Thanks! Please see #1332 :-)

---

_Comment by @AlexWaygood on 2025-11-07 04:05_

> Might be related to https://github.com/astral-sh/ty/issues/1332? But that seems to be more specifically about "mappings with the wrong types" rather than "not a mapping at all".

Specifically, this line in the example in #1332 covers your issue here:

```py
    reveal_type({**d})  # dict[Unknown, Unknown]             (good, but no diagnostic emitted!)
```

Since `d` is typed as `object` in that snippet (it is not a mapping at all)

---
