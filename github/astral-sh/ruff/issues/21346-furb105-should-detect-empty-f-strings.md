---
number: 21346
title: FURB105 should detect empty f-strings
type: issue
state: closed
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2025-11-09T16:49:06Z
updated_at: 2025-11-10T17:41:46Z
url: https://github.com/astral-sh/ruff/issues/21346
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB105 should detect empty f-strings

---

_Issue opened by @dscorbett on 2025-11-09 16:49_

### Summary

It would be helpful for [`print-empty-string` (FURB105)](https://docs.astral.sh/ruff/rules/print-empty-string/) to detect empty f-strings via [`is_empty_f_string`](https://github.com/astral-sh/ruff/blob/0.14.4/crates/ruff_python_ast/src/helpers.rs#L1414). [Example](https://play.ruff.rs/210c3a04-8fb6-43d3-a39c-4caed9335c1d):
```console
$ cat >furb105.py <<'# EOF'
print(f"")
# EOF

$ ruff --isolated check furb105.py --select FURB105
All checks passed!
```

### Version

ruff 0.14.4 (c7ff9826d 2025-11-06)

---

_Referenced in [astral-sh/ruff#21348](../../astral-sh/ruff/pulls/21348.md) on 2025-11-09 20:25_

---

_Label `rule` added by @MichaReiser on 2025-11-10 08:48_

---

_Closed by @ntBre on 2025-11-10 17:41_

---
