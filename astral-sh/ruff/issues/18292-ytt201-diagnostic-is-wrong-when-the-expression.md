```yaml
number: 18292
title: "YTT201 diagnostic is wrong when the expression uses `!=`"
type: issue
state: closed
author: dscorbett
labels:
  - diagnostics
assignees: []
created_at: 2025-05-24T21:29:12Z
updated_at: 2025-05-25T17:16:20Z
url: https://github.com/astral-sh/ruff/issues/18292
synced_at: 2026-01-12T15:54:56Z
```

# YTT201 diagnostic is wrong when the expression uses `!=`

---

_@dscorbett_

### Summary

The diagnostic for [`sys-version-info0-eq3` (YTT201)](https://docs.astral.sh/ruff/rules/sys-version-info0-eq3/) is misleading for expressions with `!=` because it is worded as if the expression uses `==`. If the expression uses `!=`, the diagnostic should recommend `<` instead of `>=`.

```console
$ cat >ytt201.py <<'# EOF'
import sys
sys.version_info[0] != 3
# EOF

$ ruff --isolated check ytt201.py --select YTT201 --output-format concise -q
ytt201.py:2:1: YTT201 `sys.version_info[0] == 3` referenced (python4), use `>=`
```

### Version

ruff 0.11.11 (0397682f1 2025-05-22)

---

_Label `diagnostics` added by @MichaReiser on 2025-05-25 10:17_

---

_Closed by @charliermarsh on 2025-05-25 17:16_

---
