```yaml
number: 1788
title: "panic: Inferring ParamSpec value...should only happen when it contains exactly one ParamSpec"
type: issue
state: closed
author: correctmost
labels:
  - fatal
  - fuzzer
assignees: []
created_at: 2025-12-05T22:16:05Z
updated_at: 2025-12-08T05:20:42Z
url: https://github.com/astral-sh/ty/issues/1788
synced_at: 2026-01-10T01:56:41Z
```

# panic: Inferring ParamSpec value...should only happen when it contains exactly one ParamSpec

---

_Issue opened by @correctmost on 2025-12-05 22:16_

### Summary

ty crashes when checking this fuzzed code:

```python
staticmethod[(),()]
```

```
error[panic]: Panicked at crates/ty_python_semantic/src/types/infer/builder.rs:3479:21 when checking `/home/user/a.py`: `Inferring ParamSpec value during explicit specialization for a tuple expression should only happen when it contains exactly one ParamSpec`
```

### Version

ty 0.0.1-alpha.32

---

_Label `fatal` added by @AlexWaygood on 2025-12-05 22:24_

---

_Label `fuzzer` added by @AlexWaygood on 2025-12-05 22:24_

---

_Assigned to @dhruvmanila by @carljm on 2025-12-05 22:48_

---

_Comment by @carljm on 2025-12-05 22:48_

@dhruvmanila can you look into this?

---

_Added to milestone `Beta` by @carljm on 2025-12-05 22:48_

---

_Closed by @dhruvmanila on 2025-12-08 05:20_

---
