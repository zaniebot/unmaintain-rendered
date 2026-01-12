```yaml
number: 8684
title: "import grouping/formating doesn't work like isort"
type: issue
state: closed
author: ArazHeydarov
labels:
  - question
assignees: []
created_at: 2023-11-14T20:40:39Z
updated_at: 2023-11-14T22:23:05Z
url: https://github.com/astral-sh/ruff/issues/8684
synced_at: 2026-01-12T15:54:48Z
```

# import grouping/formating doesn't work like isort

---

_@ArazHeydarov_

I want to use ruff as my primary format tool. Even though it fixes some other issues like, unused imports, single quotes and etc. it seems it has no effect on grouping imports.

Ruff version: 0.1.5
Tool: Mac-terminal

There are no settings for isort or ruff

Python file:
```Python
from local_file import local_module
import os
print(os)
print(local_module)
```

Command(tried both):
```console
ruff format <path_to_file>
ruff --fix <path_to_file>
```

Result:
No change

Isort command:
```console
isort <path_to_file>
```

Expected and isort result:
```Python
import os

from local_file import local_module

print(os)
print(local_module)
```


---

_Comment by @charliermarsh on 2023-11-14 21:27_

Import sorting isn't part of the default rule set, so it's not enabled by default. You can turn it on by enabling the `I` rules -- so, e.g., if you use a `ruff.toml` in your project:

```toml
[lint]
extend-select = ["I"]
```

With that, `ruff check --fix` should group and sort imports.

---

_Label `question` added by @charliermarsh on 2023-11-14 21:27_

---

_Comment by @ArazHeydarov on 2023-11-14 22:23_

Thank you for your quick response. It works now. 

---

_Closed by @ArazHeydarov on 2023-11-14 22:23_

---
