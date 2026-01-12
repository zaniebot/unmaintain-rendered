```yaml
number: 16117
title: RUF001 reports multiple violations for the same character in a nested typing string
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - rule
assignees: []
created_at: 2025-02-12T14:06:33Z
updated_at: 2025-02-12T16:27:39Z
url: https://github.com/astral-sh/ruff/issues/16117
synced_at: 2026-01-12T15:54:55Z
```

# RUF001 reports multiple violations for the same character in a nested typing string

---

_@dscorbett_

### Description

[`ambiguous-unicode-character-string` (RUF001)](https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-string/) reports one violation per level of nested quotes for a string in a typing context. This is redundant: it should only report one violation for any given character.
```console
$ cat >ruf001.py <<'# EOF'
from typing import Literal
x: '''"""'Literal["ﮨ"]'"""'''
# EOF

$ ruff check --isolated --select RUF001 ruf001.py --output-format concise
ruf001.py:2:20: RUF001 String contains ambiguous `ﮨ` (ARABIC LETTER HEH GOAL INITIAL FORM). Did you mean `o` (LATIN SMALL LETTER O)?
ruf001.py:2:20: RUF001 String contains ambiguous `ﮨ` (ARABIC LETTER HEH GOAL INITIAL FORM). Did you mean `o` (LATIN SMALL LETTER O)?
ruf001.py:2:20: RUF001 String contains ambiguous `ﮨ` (ARABIC LETTER HEH GOAL INITIAL FORM). Did you mean `o` (LATIN SMALL LETTER O)?
ruf001.py:2:20: RUF001 String contains ambiguous `ﮨ` (ARABIC LETTER HEH GOAL INITIAL FORM). Did you mean `o` (LATIN SMALL LETTER O)?
Found 4 errors.
```

---

_Label `bug` added by @AlexWaygood on 2025-02-12 16:14_

---

_Label `rule` added by @AlexWaygood on 2025-02-12 16:14_

---

_Closed by @AlexWaygood on 2025-02-12 16:27_

---
