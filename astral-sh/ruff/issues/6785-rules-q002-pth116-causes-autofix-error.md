```yaml
number: 6785
title: Rules Q002,PTH116 causes autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fixes
  - accepted
assignees: []
created_at: 2023-08-22T17:53:31Z
updated_at: 2024-03-18T01:31:26Z
url: https://github.com/astral-sh/ruff/issues/6785
synced_at: 2026-01-10T11:09:49Z
```

# Rules Q002,PTH116 causes autofix error

---

_Issue opened by @qarmin on 2023-08-22 17:53_

Ruff 0.0.285 (latest changes from main branch)

```
ruff  *.py --select Q002,PTH116 --no-cache
```

file content:
```
﻿''"assert" ' SAM macro definitions '''
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `869198225PY_FILE_TEST_7954205181857005371.py`, the rule codes Q000, Q002, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```

[869198225PY_FILE_TEST_7954205181857005371.py.zip](https://github.com/astral-sh/ruff/files/12411438/869198225PY_FILE_TEST_7954205181857005371.py.zip)



---

_Comment by @zanieb on 2023-08-22 17:57_

```
unexpected EOF while parsing at byte offset 39
---
"""assert" " SAM macro definitions "''

---
```

Since the beginning gets converted to `"""` we end up with an unterminated string literal

---

_Label `bug` added by @zanieb on 2023-08-22 17:59_

---

_Label `accepted` added by @zanieb on 2023-08-22 17:59_

---

_Label `autofix` added by @zanieb on 2023-08-22 18:11_

---

_Renamed from "Rules Q000, Q002 causes autofix error" to "Rules Q002,PTH116 causes autofix error" by @qarmin on 2023-09-02 21:54_

---

_Comment by @robincaloudis on 2024-03-02 13:26_

The underlying classes and concepts handling this quote-fixing mechanism used for inline/multiline strings as well as docstrings look interesting. I give it a shot.

---

_Comment by @robincaloudis on 2024-03-17 15:31_

Hey @zanieb, in case you are interested in the fix, I'd highly appreciate a review. I got in the habit of tagging Charlie whenever I had PR's to review, but do not want to put the pressure of reviewing on him all the time as he has probably a lot of things to do. You are a maintainer as well, aren't you? Thanks! 

---

_Comment by @zanieb on 2024-03-17 20:49_

I'll try to take a look this week.

---

_Closed by @charliermarsh on 2024-03-18 01:31_

---
