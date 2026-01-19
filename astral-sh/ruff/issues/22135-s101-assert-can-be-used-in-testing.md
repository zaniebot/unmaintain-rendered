```yaml
number: 22135
title: S101 assert can be used in testing.
type: issue
state: closed
author: bytepure
labels:
  - question
assignees: []
created_at: 2025-12-22T06:51:39Z
updated_at: 2026-01-19T08:52:21Z
url: https://github.com/astral-sh/ruff/issues/22135
synced_at: 2026-01-19T09:26:50Z
```

# S101 assert can be used in testing.

---

_@bytepure_

### Summary

https://docs.astral.sh/ruff/rules/assert/

in testing, `assert` used frequently

so, is there a way to allow the use of `assert` command in tests, and in src not allow

```
project
  -src
  -tests
```


---

_Comment by @ntBre on 2025-12-22 14:14_

I think this is a perfect use case for the [per-file-ignores](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores) setting with a value like:

```toml
[lint.per-file-ignores]
"tests/*" = ["S101"]
```

---

_Label `question` added by @ntBre on 2025-12-22 14:14_

---

_Closed by @MichaReiser on 2026-01-19 08:52_

---
