```yaml
number: 15785
title: D205 false positive from Ruff on python script
type: issue
state: open
author: vpkarle
labels:
  - rule
assignees: []
created_at: 2025-01-28T13:56:35Z
updated_at: 2025-02-02T18:44:41Z
url: https://github.com/astral-sh/ruff/issues/15785
synced_at: 2026-01-10T11:09:57Z
```

# D205 false positive from Ruff on python script

---

_Issue opened by @vpkarle on 2025-01-28 13:56_

### Description

**Summary:** 
Ruff is flagging a D205 warning (blank line required) on docstrings even though the code is compliant with the rule.
The script complies with pydocstyle checks.

**Steps to reproduce:** 
Create a python script file (myFile.py) which with below lines.
```
"""
This script processes images in a folder, extracts metadata,
and displays metadata.

See ReadMe for more details.
"""

import os
import subprocess
import sys
```
Run command "ruff check myFile.py"

**Actual output:**
```
myFile.py:1:1: D205 1 blank line required between summary line and description
  |
1 | / """
2 | | This script processes images in a folder, extracts metadata,
3 | | and displays metadata.
4 | |
5 | | See ReadMe for more details.
6 | | """
  | |___^ D205
7 |
8 |   import cv2
  |
  = help: Insert single blank line
```

**Expected behavior:** 
Ruff should not flag this docstring with a D205 warning.

**Version:** `ruff-0.9.3`

---

_Comment by @ntBre on 2025-01-28 15:38_

I can reproduce this with this code and `ruff check --select D205 d205.py`. I'll have to check the intention behind D205 to know if this is a bug or intended behavior, but I also noticed that if you put the whole first/summary line on a single line, it doesn't trigger D205:

```python
"""
This script processes images in a folder, extracts metadata, and displays metadata.

See ReadMe for more details.
"""

import os
import subprocess
import sys
```

---

_Comment by @dhruvmanila on 2025-01-29 10:58_

I think the rule only checks for first line without considering whether the line actually finishes the sentence or not. So, it's seeing that there's no blank line after "extracts metadata," which triggers the diagnostic. The PEP does specify "one-line summary" but I can't find if that allows the summary itself to be multiline, I think it doesn't. If so, then this is working as expected. What do you think @ntBre ?

---

_Label `question` added by @dhruvmanila on 2025-01-29 10:58_

---

_Comment by @ntBre on 2025-01-29 13:57_

I think you're right that it's working as intended. From [PEP 257](https://peps.python.org/pep-0257/#multi-line-docstrings):

> Multi-line docstrings consist of a summary line just like a one-line docstring, followed by a blank line, followed by a more elaborate description. The summary line may be used by automatic indexing tools; it is **important that it fits on one line** and is separated from the rest of the docstring by a blank line.

The numpy and Google style guides linked from the pydocstyle docs are similar, with [Google's](https://google.github.io/styleguide/pyguide.html#381-docstrings) probably being the clearest:

> A docstring should be organized as a summary line (one physical line not exceeding 80 characters)

---

_Comment by @MichaReiser on 2025-02-02 18:44_

What I find confusing here is that we report the entire docstring. I'd find it easier to understand if the rule highlights the `and displays metadata` part of the docstring. 

---

_Label `question` removed by @MichaReiser on 2025-02-02 18:44_

---

_Label `rule` added by @MichaReiser on 2025-02-02 18:44_

---
