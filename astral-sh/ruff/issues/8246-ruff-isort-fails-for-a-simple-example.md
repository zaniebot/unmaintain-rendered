---
number: 8246
title: Ruff isort fails for a simple example
type: issue
state: closed
author: alanwilter
labels: []
assignees: []
created_at: 2023-10-26T08:10:08Z
updated_at: 2023-10-26T08:53:21Z
url: https://github.com/astral-sh/ruff/issues/8246
synced_at: 2026-01-10T01:22:48Z
---

# Ruff isort fails for a simple example

---

_Issue opened by @alanwilter on 2023-10-26 08:10_

`ruff`: 0.1.2
`isort`: 5.10.1
```yml
[tool.ruff]
target-version = "py38"
line-length = 120
extend-select = ["UP", "RUF"]
```

I'm simply doing this and comparing with original `isort`:

```bash
cat <<EOF >|test.py
import os
import parmed as pmd
import sys
EOF
echo "Original-----"
cat test.py
echo "--------------------------"
isort test.py
echo "--------ISORT: OK --------"
cat test.py
echo "--------------------------"
ruff format test.py
echo "-----RUFF after ISORT-----"
cat test.py
echo "--------------------------"
cat <<EOF >|test.py
import os
import parmed as pmd
import sys
EOF
ruff format test.py
echo "--RUFF on Original: NO CHANGE-------------"
cat test.py
echo "--------------------------"
```

Output:
```bash
Original-----
import os
import parmed as pmd
import sys
--------------------------
Fixing /mnt/data/awilter/acpype_git/_temp/test.py
--------ISORT: OK --------
import os
import sys

import parmed as pmd
--------------------------
1 file left unchanged
-----RUFF after ISORT-----
import os
import sys

import parmed as pmd
--------------------------
1 file left unchanged
--RUFF on Original: NO CHANGE-------------
import os
import parmed as pmd
import sys
--------------------------
```

Interestingly, `VScode` with `ruff-vscode` extension works fine.

<img width="570" alt="Screenshot 2023-10-26 at 09 15 02" src="https://github.com/astral-sh/ruff/assets/3899850/6bcd786f-053b-42f7-933b-d9d548f53754">
<img width="313" alt="Screenshot 2023-10-26 at 09 15 34" src="https://github.com/astral-sh/ruff/assets/3899850/8f650fd6-b635-45ed-aff5-3b4611b54371">




---

_Comment by @MichaReiser on 2023-10-26 08:11_

I believe you need to enable isort manually by including `"I"` in `extend-select`.

---

_Comment by @alanwilter on 2023-10-26 08:40_

Nope, that doesn't change anything.
I'm actually annoyed I have to add "RUF" and "UP". I was expecting `ruff` to do it all by default.

---

_Comment by @MichaReiser on 2023-10-26 08:43_

Oh, `isort` is parth of `check` and not `format`. Can you try running `ruff check --fix`?

---

_Comment by @alanwilter on 2023-10-26 08:53_

I had to update my test, and do `extend-select = ["UP", "RUF", "I"]`:

```bash
cat <<EOF >|test.py
import os
import parmed as pmd
import sys
print(os,sys,pmd)
EOF
ruff format test.py
echo "--RUFF on Original: NO CHANGE-------------"
cat test.py
echo "--------------------------"
ruff check test.py --fix
echo "--RUFF check on Original: OK-------------"
cat test.py
echo "--------------------------"
```
And now, you're right, it's working.

I will close this ticket.


---

_Closed by @alanwilter on 2023-10-26 08:53_

---
