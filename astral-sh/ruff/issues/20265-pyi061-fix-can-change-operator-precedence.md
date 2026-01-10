```yaml
number: 20265
title: PYI061 fix can change operator precedence
type: issue
state: closed
author: dscorbett
labels:
  - fixes
assignees: []
created_at: 2025-09-05T12:31:20Z
updated_at: 2025-10-15T15:06:04Z
url: https://github.com/astral-sh/ruff/issues/20265
synced_at: 2026-01-10T11:09:59Z
```

# PYI061 fix can change operator precedence

---

_Issue opened by @dscorbett on 2025-09-05 12:31_

### Summary

The fix for [`redundant-none-literal` (PYI061)](https://docs.astral.sh/ruff/rules/redundant-none-literal/) in Python 3.10 and later changes a subscription into a `|` expression, which has different precedence, which can change the program’s behavior outside of typing contexts. The fix should insert parentheses when necessary to maintain the correct precedence, i.e. `(Literal[1] | None).__dict__` in [this example](https://play.ruff.rs/4622390b-e4a0-44c0-bce3-e592c36c1f6d):
```console 
$ cat >pyi061.py <<'# EOF'
from typing import Literal
print(Literal[1, None].__dict__)
# EOF

$ python pyi061.py
{'_inst': True, '_name': None, '__origin__': typing.Literal, '__slots__': None, '__args__': (1, None), '__parameters__': (), '__module__': 'typing'}

$ ruff --isolated check pyi061.py --select PYI061 --preview --target-version py310 --fix
Found 1 error (1 fixed, 0 remaining).

$ cat pyi061.py 
from typing import Literal
print(Literal[1] | None.__dict__)

$ python pyi061.py 2>&1 | tail -n 1
AttributeError: 'NoneType' object has no attribute '__dict__'. Did you mean: '__dir__'?
```

### Version

ruff 0.12.12 (c6516e9b6 2025-09-04)

---

_Comment by @AlexWaygood on 2025-09-05 12:38_

Thanks -- as always! While this is a valid bug report, I don't think this one should block stabilising the rule since I think it's very unlikely that a human, LLM or code generation tool would ever produce code like this

---

_Label `fixes` added by @AlexWaygood on 2025-09-05 12:38_

---

_Closed by @ntBre on 2025-10-15 15:06_

---
