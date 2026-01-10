```yaml
number: 20456
title: "SIM905 diagnostic mentions `str.split` instead of `str.rsplit`"
type: issue
state: closed
author: dscorbett
labels:
  - good first issue
  - diagnostics
assignees: []
created_at: 2025-09-17T19:44:13Z
updated_at: 2025-09-18T07:52:09Z
url: https://github.com/astral-sh/ruff/issues/20456
synced_at: 2026-01-10T11:09:59Z
```

# SIM905 diagnostic mentions `str.split` instead of `str.rsplit`

---

_Issue opened by @dscorbett on 2025-09-17 19:44_

### Summary

The diagnostic for [`split-static-string` (SIM905)](https://docs.astral.sh/ruff/rules/split-static-string/) mentions `str.split` even when the relevant method is `str.rsplit`. [Example](https://play.ruff.rs/730e5360-1356-485f-848e-c8ebda0b9ebc):
```console
$ cat >sim905.py <<'# EOF'
"a,b,c,d".rsplit(",")
# EOF

$ ruff --isolated check sim905.py --select SIM905 --output-format concise -q
sim905.py:1:1: SIM905 [*] Consider using a list literal instead of `str.split`
```

### Version

ruff 0.13.0 (a1fdd66f1 2025-09-10)

---

_Label `good first issue` added by @ntBre on 2025-09-17 19:46_

---

_Label `diagnostics` added by @ntBre on 2025-09-17 19:46_

---

_Closed by @MichaReiser on 2025-09-18 07:52_

---
