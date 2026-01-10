```yaml
number: 2194
title: RUF100 without selecting the specific error codes
type: issue
state: closed
author: spaceone
labels:
  - question
assignees: []
created_at: 2023-01-26T14:23:35Z
updated_at: 2023-02-03T18:24:05Z
url: https://github.com/astral-sh/ruff/issues/2194
synced_at: 2026-01-10T11:09:45Z
```

# RUF100 without selecting the specific error codes

---

_Issue opened by @spaceone on 2023-01-26 14:23_

`RUF100`  removes `noqa` comments if the lines doesn't have the underlying issue.

This doesn't work if the certain errors aren't part of the `--select`. I wouldn't have expect this:
```sh
$ cat foo.py
from foo import bar  # noqa: F401

foo = lambda x: None  # noqa: E731

import os  # noqa: E403

reload(os)  # noqa: F821
$ ruff --isolated --select RUF100 foo.py
foo.py:1:22: RUF100 Unused `noqa` directive (non-enabled: `F401`)
foo.py:3:23: RUF100 Unused `noqa` directive (non-enabled: `E731`)
foo.py:5:12: RUF100 Unused `noqa` directive (unknown: `E403`)
foo.py:7:13: RUF100 Unused `noqa` directive (non-enabled: `F821`)
Found 4 errors.
4 potentially fixable with the --fix option.
$ ruff --isolated --select RUF100 --fix foo.py
Found 4 errors (4 fixed, 0 remaining).
$ cat foo.py
from foo import bar

foo = lambda x: None

import os

reload(os)
```

I can also only select codes which ruff is aware of:
```sh
$ ruff --isolated --select RUF100,F401,E731,F821 foo.py
foo.py:5:12: RUF100 Unused `noqa` directive (unknown: `E403`)
Found 1 error.
1 potentially fixable with the --fix option.
$ ruff --isolated --select RUF100,F401,E731,F821,E403 foo.py
error: Invalid value 'E403' for '--select <RULE_CODE>': Unknown rule selector `E403`

For more information try '--help'
```

`E403` is not yet supported therefore this is always removed and one cannot prevent this.

---

_Comment by @charliermarsh on 2023-01-26 14:46_

You can add `E403` to "external" to prevent this, I think?

```toml
[tool.ruff]
external = ["E403"]
```

---

_Label `question` added by @charliermarsh on 2023-01-26 14:53_

---

_Comment by @charliermarsh on 2023-01-26 14:54_

I'd recommend running `ruff --extend-select RUF100`. That way, it'll enforce your standard configuration.

---

_Comment by @spaceone on 2023-01-26 15:08_

I somehow need a way to exclusively select `RUF100` so that I can get a list of issues (and fixes) only for that one.

---

_Comment by @spaceone on 2023-01-26 15:18_

fixing only is possible via:
`ruff --select ALL --extend-select RUF100 --fix-only --unfixable ALL --fixable RUF100 $(python_files)`

selecting only is possible via:
`ruff --select ALL --extend-select RUF100 $(python_files) | grep RUF100`

(with optional `--select ALL`)

---

_Closed by @charliermarsh on 2023-02-03 18:24_

---
