```yaml
number: 2125
title: Before filing an issue
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2025-12-20T00:31:18Z
updated_at: 2025-12-23T11:22:12Z
url: https://github.com/astral-sh/ty/issues/2125
synced_at: 2026-01-10T01:56:41Z
```

# Before filing an issue

---

_Issue opened by @carljm on 2025-12-20 00:31_

## Thanks for trying ty and reporting an issue!

**First, please check whether your question or problem is already known.**

1. If you are seeing a type error on your code that you didn't expect, does the [documentation reference page](https://docs.astral.sh/ty/reference/rules/) for that error code explain the problem?
2. Have we documented it in our [typing FAQ](https://docs.astral.sh/ty/reference/typing-faq/)?
3. Is it a [typing feature](https://github.com/astral-sh/ty/issues/1889) we haven't (fully) implemented yet?
4. Or is it one of these known bugs?
- Sometimes ty fails to narrow types as it should when there is a call to a `NoReturn` or `Never` returning function (e.g. `sys.exit()`) in a nested control flow path. This is tracked in https://github.com/astral-sh/ty/issues/690
- ty may not infer a specialization for a type variable inside a generic protocol. This is tracked in https://github.com/astral-sh/ty/issues/1714


---

_Locked by @astral-sh on 2025-12-23 11:22_

---
