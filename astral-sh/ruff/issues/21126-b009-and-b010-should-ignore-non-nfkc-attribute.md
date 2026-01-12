```yaml
number: 21126
title: B009 and B010 should ignore non-NFKC attribute names
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-10-29T17:22:24Z
updated_at: 2025-11-03T14:45:24Z
url: https://github.com/astral-sh/ruff/issues/21126
synced_at: 2026-01-12T15:54:57Z
```

# B009 and B010 should ignore non-NFKC attribute names

---

_@dscorbett_

### Summary

[`get-attr-with-constant` (B009)](https://docs.astral.sh/ruff/rules/get-attr-with-constant/) and [`set-attr-with-constant` (B010)](https://docs.astral.sh/ruff/rules/set-attr-with-constant/) should ignore non-NFKC attribute names because the usual attribute syntax would change the program’s behavior. [Example](https://play.ruff.rs/8beed5a5-02f2-40df-9533-71d04176ac50):
```console
$ cat >b009_b010.py <<'# EOF'
from types import SimpleNamespace
ns = SimpleNamespace(s=0)
setattr(ns, "ſ", 1)
print(ns.s, getattr(ns, "ſ"))
# EOF

$ python b009_b010.py
0 1

$ ruff check --isolated b009_b010.py --select B009,B010 --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat b009_b010.py
from types import SimpleNamespace
ns = SimpleNamespace(s=0)
ns.ſ = 1
print(ns.s, ns.ſ)

$ python b009_b010.py
1 1
```

### Version

ruff 0.14.2 (83a3bc4ee 2025-10-23)

---

_Label `bug` added by @ntBre on 2025-10-29 18:07_

---

_Closed by @MichaReiser on 2025-11-03 14:45_

---
