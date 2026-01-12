```yaml
number: 20238
title: "RUF060 analyzes `f\"\" \"1\"` as empty"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - preview
assignees: []
created_at: 2025-09-04T15:05:47Z
updated_at: 2025-09-04T20:21:00Z
url: https://github.com/astral-sh/ruff/issues/20238
synced_at: 2026-01-12T15:54:57Z
```

# RUF060 analyzes `f"" "1"` as empty

---

_@dscorbett_

### Summary

[`in-empty-collection` (RUF060)](https://docs.astral.sh/ruff/rules/in-empty-collection/) analyzes a non-empty f-string as empty when it is implicitly concatenated and all the pieces with the `f` prefix are empty but one of the pieces without the `f` prefix is not empty. [Example](https://play.ruff.rs/8eab716f-6c29-47e8-bf54-c60bc101e5c0):
```console
$ cat >ruf060.py <<'# EOF'
"1" in f"" "1"
# EOF

$ ruff --isolated check ruf060.py --select RUF060 --preview --output-format concise -q
ruf060.py:1:1: RUF060 Unnecessary membership test on empty collection
```

The helper function [`is_empty_f_string`](https://github.com/astral-sh/ruff/blob/0.12.11/crates/ruff_python_ast/src/helpers.rs#L1409) could be useful.

### Version

ruff 0.12.11 (c2bc15bc1 2025-08-28)

---

_Label `bug` added by @ntBre on 2025-09-04 17:37_

---

_Label `preview` added by @ntBre on 2025-09-04 17:38_

---

_Comment by @TaKO8Ki on 2025-09-04 18:05_

I'll take this issue next 🚀 

---

_Closed by @dylwil3 on 2025-09-04 20:21_

---
