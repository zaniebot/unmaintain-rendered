---
number: 844
title: First-party imports not being sorted
type: issue
state: closed
author: tekumara
labels:
  - bug
assignees: []
created_at: 2022-11-21T01:32:10Z
updated_at: 2022-11-21T02:59:38Z
url: https://github.com/astral-sh/ruff/issues/844
synced_at: 2026-01-07T13:12:14-06:00
---

# First-party imports not being sorted

---

_Issue opened by @tekumara on 2022-11-21 01:32_

_myapp/main.py_:
```python
import myapp.module
import pydantic

print(pydantic.version)
print(myapp.module.hello)
```

_pyproject.toml_
```toml
[tool.ruff]
line-length = 120

# rules to enable/ignore
select = [
    # isort
    "I"
]
# first-party imports for sorting
src = ["myapp"]

fix = true
```

When I run `ruff myapp/main.py` it preserves the sort order above instead of sorting myapp last like isort does, eg:
```python
import pydantic

import myapp.module
```


ruff 0.0.132

---

_Label `bug` added by @charliermarsh on 2022-11-21 02:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-21 02:16_

---

_Comment by @charliermarsh on 2022-11-21 02:18_

Confirmed bug!

---

_Comment by @charliermarsh on 2022-11-21 02:22_

I think in this case you actually want:

```toml
src = ["."]
```

Since those are really source _roots_, so if you have `src = ["myapp"]`, and you `import myapp.module`, you're effectively looking for `./myapp/myapp/module.py`. Does that fix it?


---

_Comment by @tekumara on 2022-11-21 02:59_

OIC, yes that works, thank you! 

---

_Closed by @tekumara on 2022-11-21 02:59_

---

_Comment by @charliermarsh on 2022-11-21 02:59_

Sorry for the confusion!

---
