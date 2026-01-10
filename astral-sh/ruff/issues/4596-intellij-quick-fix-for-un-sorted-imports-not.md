```yaml
number: 4596
title: Intellij quick-fix for un-sorted imports not working
type: issue
state: closed
author: HansAarneLiblik
labels: []
assignees: []
created_at: 2023-05-23T10:47:28Z
updated_at: 2023-05-23T12:57:42Z
url: https://github.com/astral-sh/ruff/issues/4596
synced_at: 2026-01-10T11:09:47Z
```

# Intellij quick-fix for un-sorted imports not working

---

_Issue opened by @HansAarneLiblik on 2023-05-23 10:47_

I'm seeing an issue with ruff and quick-fixes it offers to Intellij

I have the following file
```py
from typing import assert_type, assert_never


if __name__ == '__main__':
    assert_never("test")
    assert_type(123, int)
```

and `ruff` complains about imports

![image](https://github.com/charliermarsh/ruff/assets/5418540/1b26ab2a-1790-49a7-8d18-be990b521765)


When I click `organize imports`, it breaks the code

```py
ffrom typing import assert_never, assert_type

f __name__ == '__main__':
    assert_never("test")
    assert_type(123, int)
```

Executing the fix from command line works `ruff . --fix`

My config from `pyproject.toml`
```toml
[tool.isort]
profile = "black"
line_length = 120
filter_files = true


[tool.ruff]
line-length = 120
target-version = "py310"
select = [
  "E",    # pycodestyle
  "W",    # pycodestyle
  "F",    # pyflakes
  "I",    # isort
  "C4",   # flake8-comprehensions
  "TID",  # flake8-tidy-imports
]
```

Ruff version is `0.0.269` and I'm using the Ruff plugin for IntelliJ IDEs


---

_Comment by @HansAarneLiblik on 2023-05-23 10:49_

Probably related, but same-ish thing happens when I have comment directly after imports

```py
from typing import assert_type, assert_never

# My comment, do not touch


if __name__ == '__main__':
    assert_never("test")
    assert_type(123, int)
```

changes to 

```py
ffrom typing import assert_never, assert_type

 My comment, do not touch


if __name__ == '__main__':
    assert_never("test")
    assert_type(123, int)
```

---

_Comment by @HansAarneLiblik on 2023-05-23 11:25_

Re-posted to Ruff IntelliJ plugin, as that may be at fault - https://github.com/koxudaxi/ruff-pycharm-plugin/issues/180

---

_Comment by @charliermarsh on 2023-05-23 12:57_

I think the PyCharm plugin needs to update to 1 offsets to support newer Ruff versions. I'll comment in the issue.

---

_Closed by @charliermarsh on 2023-05-23 12:57_

---
