```yaml
number: 15725
title: "FURB122 false positive when the target variable is used beyond the `write` call"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-01-24T16:26:03Z
updated_at: 2025-01-28T19:16:23Z
url: https://github.com/astral-sh/ruff/issues/15725
synced_at: 2026-01-12T15:54:54Z
```

# FURB122 false positive when the target variable is used beyond the `write` call

---

_@dscorbett_

### Description

[`for-loop-writes` (FURB122)](https://docs.astral.sh/ruff/rules/for-loop-writes/) in Ruff 0.9.3 has a false positive if writing to the target variable has an effect beyond the `write` call. This can happen in two ways.

When the `for` loop’s target list includes a global or nonlocal variable, writing to the target has a side effect. Rewriting the loop as a generator puts the target in a new, nested scope, which can change program behavior.
```console
$ cat >furb122_1.py <<'EOF'
from pathlib import Path
CURRENT_LINE = "..."
def wrap_line():
    return f"<{CURRENT_LINE}>\n"
def write(lines):
    with Path("furb122.txt").open("w") as f:
        global CURRENT_LINE
        for CURRENT_LINE in lines:
            f.write(wrap_line())
write(["line1", "line2"])
EOF

$ python furb122_1.py

$ cat furb122.txt
<line1>
<line2>

$ ruff --isolated check --preview --select FURB122 furb122_1.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb122_1.py
from pathlib import Path
CURRENT_LINE = "..."
def wrap_line():
    return f"<{CURRENT_LINE}>\n"
def write(lines):
    with Path("furb122.txt").open("w") as f:
        global CURRENT_LINE
        f.writelines(wrap_line() for CURRENT_LINE in lines)
write(["line1", "line2"])

$ python furb122_1.py

$ cat furb122.txt
<...>
<...>
```

Similarly, when the `for` loop’s target list includes a variable that is used after the loop, rewriting the loop as a generator changes the variable’s scope, so the outer variable is not set.
```console
$ cat >furb122_2.py <<'EOF'
from pathlib import Path
line = "..."
with Path("furb122.txt").open("w") as f:
    for line in ["line1", "line2"]:
        f.write(f"{line}\n")
print(f"{line=}")
EOF

$ python furb122_2.py
line='line2'

$ ruff --isolated check --preview --select FURB122 furb122_2.py --fix
Found 1 error (1 fixed, 0 remaining).

$ cat furb122_2.py
from pathlib import Path
line = "..."
with Path("furb122.txt").open("w") as f:
    f.writelines(f"{line}\n" for line in ["line1", "line2"])
print(f"{line=}")

$ python furb122_2.py
line='...'
```

---

_Label `bug` added by @AlexWaygood on 2025-01-24 16:37_

---

_Label `fixes` added by @AlexWaygood on 2025-01-24 16:37_

---

_Closed by @AlexWaygood on 2025-01-28 19:16_

---
