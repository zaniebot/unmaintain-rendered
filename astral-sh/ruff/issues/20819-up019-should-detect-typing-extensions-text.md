```yaml
number: 20819
title: "UP019 should detect `typing_extensions.Text`"
type: issue
state: closed
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2025-10-12T14:29:43Z
updated_at: 2025-10-15T06:52:15Z
url: https://github.com/astral-sh/ruff/issues/20819
synced_at: 2026-01-10T11:09:59Z
```

# UP019 should detect `typing_extensions.Text`

---

_Issue opened by @dscorbett on 2025-10-12 14:29_

### Summary

[`typing-text-str-alias` (UP019)](https://docs.astral.sh/ruff/rules/typing-text-str-alias/) detects `typing.Text`. It would also be useful for it to detect `typing_extensions.Text`. [Example](https://play.ruff.rs/c9b704ed-1e1e-45aa-8e5a-de32fd3280ac):
```console
$ cat >up019.py <<'# EOF'
from typing_extensions import Text
foo: Text = "bar"
# EOF

$ ruff --isolated check up019.py --select UP019
All checks passed!
```

### Version

ruff 0.14.0 (beea8cdfe 2025-10-07)

---

_Label `rule` added by @ntBre on 2025-10-13 13:06_

---

_Closed by @MichaReiser on 2025-10-15 06:52_

---
