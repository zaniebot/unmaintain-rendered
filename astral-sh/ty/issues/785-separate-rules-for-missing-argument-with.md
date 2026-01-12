```yaml
number: 785
title: "Separate rules for missing-argument with `**`"
type: issue
state: closed
author: adamh-oai
labels: []
assignees: []
created_at: 2025-07-08T20:52:24Z
updated_at: 2025-07-09T06:19:52Z
url: https://github.com/astral-sh/ty/issues/785
synced_at: 2026-01-12T15:54:24Z
```

# Separate rules for missing-argument with `**`

---

_@adamh-oai_

### Summary

One of the most common type errors reported by ty in our codebase looks like:

```
error[missing-argument]: No arguments provided for required parameters `foo`, `bar`
    --> src/some/thing.py
     |
1055 |             user = SomeClass(**row)
     |                    ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
     |
info: rule `missing-argument` is enabled by default
```

My understanding is that pyright and mypy allow this without checking the arguments.   It would be helpful for ty to expose different rules for a missing-arguments with a normal function call, and missing-arguments-with-splat when there's a ** involved.  Ideally we'd be clean for both, but initially would disable the -with-splat version while enabling the regular missing-arguments.

### Version

ty 0.0.1-alpha.14 (3ececb07e 2025-07-08)

---

_Comment by @sharkdp on 2025-07-09 06:19_

Thank you for reporting this. We don't support splatted arguments at all, right now. Please see #247 

---

_Closed by @sharkdp on 2025-07-09 06:19_

---
