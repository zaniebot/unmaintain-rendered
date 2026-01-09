---
number: 2105
title: "F821 on a `Literal` type that isn't directly imported by the current module"
type: issue
state: closed
author: joerick
labels: []
assignees: []
created_at: 2023-01-23T09:54:20Z
updated_at: 2023-01-23T20:08:50Z
url: https://github.com/astral-sh/ruff/issues/2105
synced_at: 2026-01-07T13:12:14-06:00
---

# F821 on a `Literal` type that isn't directly imported by the current module

---

_Issue opened by @joerick on 2023-01-23 09:54_

We're getting a F821 on a Literal type over in https://github.com/pypa/cibuildwheel/pull/1405 . I've isolated this to the following-

Two files are needed-

**script.py**
```python
from __future__ import annotations

from script_typing import Literal

testing_archs: list[Literal["x86_64", "arm64"]]
testing_archs = ['x86_64']
print(testing_archs)
```

**script_typing.py**
```python
import sys

if sys.version_info < (3, 8):
    from typing_extensions import Literal
else:
    from typing import Literal
```

Here's what happens:
```sh
$ ruff --isolated --fix script.py 
script.py:5:29: F821 Undefined name `x86_64`
script.py:5:39: F821 Undefined name `arm64`
Found 2 error(s).
```

This happens without any pyproject.toml configuration

Version: `ruff 0.0.230`.

*p.s. ruff seems pretty incredible! the speed 🤯*

---

_Referenced in [pypa/cibuildwheel#1405](../../pypa/cibuildwheel/pulls/1405.md) on 2023-01-23 10:01_

---

_Comment by @sbrugman on 2023-01-23 13:32_

There is an option that should help in this case: https://github.com/charliermarsh/ruff#typing-modules

```
[tool.ruff]
typing-modules = ["cibuildwheel.typing"]
```

(Which I see you are already using, but is not fixing the issue )

---

_Comment by @joerick on 2023-01-23 13:53_

Ah, thanks for pointing that out, I suppose @henryiii found that option before I did!

Then I think the issue is perhaps the relative import. See this new recreation:

 -  📁 `mymodule`

     -  📄 `__init__.py`
     -  📄 `script.py`

        ```python
        from __future__ import annotations

		from .script_typing import Literal

		testing_archs: list[Literal["x86_64", "arm64"]]
		testing_archs = ['x86_64']
		print(testing_archs)
		```
	 -  📄 `script_typing.py`

	    ```python
	    import sys

		if sys.version_info < (3, 8):
		    from typing_extensions import Literal
		else:
		    from typing import Literal

		__all__ = ['Literal']

		```

 -  📄 `pyproject.toml`

    ```toml
    [tool.ruff]
	typing-modules = ['mymodule.script_typing']
	```

```
$ ruff mymodule                     
mymodule/script.py:5:29: F821 Undefined name `x86_64`
mymodule/script.py:5:39: F821 Undefined name `arm64`
Found 2 error(s).
```


---

_Comment by @joerick on 2023-01-23 13:54_

In this version, changing the import in script.py to `from mymodule.script_typing import Literal` silences the error.

---

_Comment by @henryiii on 2023-01-23 14:18_

Dupe #2005

---

_Comment by @joerick on 2023-01-23 14:25_

Ah, I didn't know enough about typing-modules to realise this was dupe. Apologies!

---

_Closed by @joerick on 2023-01-23 14:25_

---

_Comment by @charliermarsh on 2023-01-23 17:41_

All good, thanks everyone for helping out here!

---
