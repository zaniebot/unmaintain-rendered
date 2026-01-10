```yaml
number: 16579
title: Many fixes normalize string literals unnecessarily
type: issue
state: open
author: dscorbett
labels:
  - fixes
  - wish
assignees: []
created_at: 2025-03-09T17:45:58Z
updated_at: 2025-03-10T08:41:26Z
url: https://github.com/astral-sh/ruff/issues/16579
synced_at: 2026-01-10T11:09:57Z
```

# Many fixes normalize string literals unnecessarily

---

_Issue opened by @dscorbett on 2025-03-09 17:45_

### Summary

Multiple rules’ fixes normalize string literals’ escape sequences. Fixes should keep string literals as unchanged as possible when normalizing them is not relevant to the rule. The examples below demonstrate this by making the fixes introduce [`ambiguous-unicode-character-string` (RUF001)](https://docs.astral.sh/ruff/rules/ambiguous-unicode-character-string/) violations, but this isn’t just about ambiguous Unicode characters: all unnecessary normalizations should be avoided.

[`set-attr-with-constant` (B010)](https://docs.astral.sh/ruff/rules/set-attr-with-constant/):
```console
$ cat >b010.py <<'# EOF'
from types import SimpleNamespace
x = SimpleNamespace()
setattr(x, "x", "\u2216")
# EOF

$ ruff --isolated check --select B010 b010.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat b010.py
from types import SimpleNamespace
x = SimpleNamespace()
x.x = "∖"

$ ruff --isolated check --select RUF001 b010.py --output-format concise
b010.py:3:8: RUF001 String contains ambiguous `∖` (SET MINUS). Did you mean `\` (REVERSE SOLIDUS)?
Found 1 error.
```

[`unnecessary-dict-comprehension-for-iterable` (C420)](https://docs.astral.sh/ruff/rules/unnecessary-dict-comprehension-for-iterable/):
```console
$ cat >c420.py <<'# EOF'
{x: "\U0001051C" for x in "\u03F9\u0421"}
# EOF

$ ruff --isolated check --preview --select C420 c420.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat c420.py
dict.fromkeys("ϹС", "𐔜")

$ ruff --isolated check --select RUF001 c420.py --output-format concise
c420.py:1:16: RUF001 String contains ambiguous `Ϲ` (GREEK CAPITAL LUNATE SIGMA SYMBOL). Did you mean `C` (LATIN CAPITAL LETTER C)?
c420.py:1:17: RUF001 String contains ambiguous `С` (CYRILLIC CAPITAL LETTER ES). Did you mean `C` (LATIN CAPITAL LETTER C)?
c420.py:1:22: RUF001 String contains ambiguous `𐔜` (ELBASAN LETTER SHE). Did you mean `C` (LATIN CAPITAL LETTER C)?
Found 3 errors.
```

[`static-join-to-f-string` (FLY002)](https://docs.astral.sh/ruff/rules/static-join-to-f-string/):
```console
$ cat >fly002.py <<'# EOF'
print("\u2E12".join(("\u0395", "\u0399", "\u0395", "\u0399", "\u039F")))
# EOF

$ ruff --isolated check --select FLY002 fly002.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat fly002.py
print("Ε⸒Ι⸒Ε⸒Ι⸒Ο")

$ ruff --isolated check --select RUF001 fly002.py --output-format concise
fly002.py:1:8: RUF001 String contains ambiguous `Ε` (GREEK CAPITAL LETTER EPSILON). Did you mean `E` (LATIN CAPITAL LETTER E)?
fly002.py:1:10: RUF001 String contains ambiguous `Ι` (GREEK CAPITAL LETTER IOTA). Did you mean `I` (LATIN CAPITAL LETTER I)?
fly002.py:1:12: RUF001 String contains ambiguous `Ε` (GREEK CAPITAL LETTER EPSILON). Did you mean `E` (LATIN CAPITAL LETTER E)?
fly002.py:1:14: RUF001 String contains ambiguous `Ι` (GREEK CAPITAL LETTER IOTA). Did you mean `I` (LATIN CAPITAL LETTER I)?
fly002.py:1:16: RUF001 String contains ambiguous `Ο` (GREEK CAPITAL LETTER OMICRON). Did you mean `O` (LATIN CAPITAL LETTER O)?
Found 5 errors.
```

[`pytest-parametrize-names-wrong-type` (PT006)](https://docs.astral.sh/ruff/rules/pytest-parametrize-names-wrong-type/):
```console
$ cat >pt006.py <<'# EOF'
import pytest
@pytest.mark.parametrize("\u0561,\u03B3", [(1, 3), (0.739, 0.577)])
def test_inequality(ա, γ):
    assert ա != γ
# EOF

$ ruff --isolated check --select PT006 pt006.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat pt006.py
import pytest
@pytest.mark.parametrize(("ա", "γ"), [(1, 3), (0.739, 0.577)])
def test_inequality(ա, γ):
    assert ա != γ

$ ruff --isolated check --select RUF001 pt006.py --output-format concise
pt006.py:2:28: RUF001 String contains ambiguous `ա` (ARMENIAN SMALL LETTER AYB). Did you mean `w` (LATIN SMALL LETTER W)?
pt006.py:2:33: RUF001 String contains ambiguous `γ` (GREEK SMALL LETTER GAMMA). Did you mean `y` (LATIN SMALL LETTER Y)?
Found 2 errors.
```

[`unnecessary-regular-expression` (RUF055)](https://docs.astral.sh/ruff/rules/unnecessary-regular-expression/):
```console
$ cat >ruf055.py <<'# EOF'
import re
re.sub("\12", "\x5e", """\
\u03B1
\u039C""")
re.split(U"\U0001D114", u"\N{musical symbol G clef}\N{musical symbol brace}\N{musical symbol F clef}")
# EOF

$ ruff --isolated check --preview --select RUF055 ruf055.py --fix
Found 2 errors (2 fixed, 0 remaining).

$ cat ruf055.py
import re
"""α\nΜ""".replace("\n", "^")
u"𝄞𝄔𝄢".split(u"𝄔")

$ ruff --isolated check --select RUF001 ruf055.py --output-format concise
ruf055.py:2:4: RUF001 String contains ambiguous `α` (GREEK SMALL LETTER ALPHA). Did you mean `a` (LATIN SMALL LETTER A)?
ruf055.py:2:7: RUF001 String contains ambiguous `Μ` (GREEK CAPITAL LETTER MU). Did you mean `M` (LATIN CAPITAL LETTER M)?
ruf055.py:3:4: RUF001 String contains ambiguous `𝄔` (MUSICAL SYMBOL BRACE). Did you mean `{` (LEFT CURLY BRACKET)?
ruf055.py:3:16: RUF001 String contains ambiguous `𝄔` (MUSICAL SYMBOL BRACE). Did you mean `{` (LEFT CURLY BRACKET)?
Found 4 errors.
```

[`uncapitalized-environment-variables` (SIM112)](https://docs.astral.sh/ruff/rules/uncapitalized-environment-variables/):
```console
$ cat >sim112.py <<'# EOF'
import os
os.environ["x\xd7"]
# EOF

$ ruff --isolated check --select SIM112 sim112.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat sim112.py
import os
os.environ["X×"]

$ ruff --isolated check --select RUF001 sim112.py --output-format concise
sim112.py:2:14: RUF001 String contains ambiguous `×` (MULTIPLICATION SIGN). Did you mean `x` (LATIN SMALL LETTER X)?
Found 1 error.
```

[`split-static-string` (SIM905)](https://docs.astral.sh/ruff/rules/split-static-string/):
```console
$ cat >sim905.py <<'# EOF'
"\uFBA8\u0640\uFBA7".split("\U00000640")
# EOF

$ ruff --isolated check --preview --select SIM905 sim905.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat sim905.py
["ﮨ", "ﮧ"]

$ ruff --isolated check --select RUF001 sim905.py --output-format concise
sim905.py:1:3: RUF001 String contains ambiguous `ﮨ` (ARABIC LETTER HEH GOAL INITIAL FORM). Did you mean `o` (LATIN SMALL LETTER O)?
sim905.py:1:8: RUF001 String contains ambiguous `ﮧ` (ARABIC LETTER HEH GOAL FINAL FORM). Did you mean `o` (LATIN SMALL LETTER O)?
Found 2 errors.
```

[`typing-only-standard-library-import` (TC003)](https://docs.astral.sh/ruff/rules/typing-only-standard-library-import/):
```console
$ cat >tc003.py <<'# EOF'
from collections.abc import Sequence
from typing import Literal
U: Sequence[Literal["\N{UNION}"]]
# EOF

$ ruff --isolated check --select TC003 tc003.py --config 'lint.flake8-type-checking.quote-annotations = true' --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat tc003.py
from typing import Literal, TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Sequence
U: "Sequence[Literal['∪']]"

$ ruff --isolated check --select RUF001 tc003.py --output-format concise
tc003.py:5:23: RUF001 String contains ambiguous `∪` (UNION). Did you mean `U` (LATIN CAPITAL LETTER U)?
Found 1 error.
```

### Version

ruff 0.9.10 (0dfa810e9 2025-03-07)

---

_Label `fixes` added by @AlexWaygood on 2025-03-09 17:55_

---

_Label `wish` added by @MichaReiser on 2025-03-10 08:41_

---

_Comment by @MichaReiser on 2025-03-10 08:41_

While I think this is desired, I don't consider it a bug. 

---
