```yaml
number: 2253
title: "--add-noqa adds duplicated noqa comment"
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-01-27T14:23:59Z
updated_at: 2023-02-02T02:58:03Z
url: https://github.com/astral-sh/ruff/issues/2253
synced_at: 2026-01-12T15:54:42Z
```

# --add-noqa adds duplicated noqa comment

---

_@spaceone_

```sh
$ cat foo.py
from foo import (  # noqa: F401
        bar
)
$ ruff --select F401 --add-noqa foo.py
Added 1 noqa directives.
$ cat foo.py
from foo import (  # noqa: F401
        bar  # noqa: F401
)

```

---

_Comment by @charliermarsh on 2023-01-27 14:26_

Ah thank you. I probably didn't extend `--add-noqa` to handle those "parent-style" comments when we added support for them.

---

_Comment by @charliermarsh on 2023-01-27 14:27_

Does `--select RUF100 --fix` remove the dupe? (Still a bug.)

---

_Label `bug` added by @charliermarsh on 2023-01-27 14:27_

---

_Comment by @spaceone on 2023-01-27 14:43_

yes, `ruff --select RUF100,F401 --fix foo.py` restores the original version

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-02 02:45_

---

_Closed by @charliermarsh on 2023-02-02 02:58_

---
