---
number: 15947
title: "Incorrectly raised error F821 Undefined name `migrations`"
type: issue
state: closed
author: softwaredevPK
labels: []
assignees: []
created_at: 2025-02-04T20:28:29Z
updated_at: 2025-02-04T21:53:02Z
url: https://github.com/astral-sh/ruff/issues/15947
synced_at: 2026-01-07T13:12:16-06:00
---

# Incorrectly raised error F821 Undefined name `migrations`

---

_Issue opened by @softwaredevPK on 2025-02-04 20:28_

### Description

I have got an error made by ruff that shouldn't be there.

command: `ruff check $PROJECT_NAME --select A,B,C,E,F --ignore B026,C901`
pyproject.toml
`
[tool.black]
line-length = 120
target-version = ['py38']

[tool.ruff]
line-length = 120

[tool.ruff.lint.per-file-ignores]
"**/{migrations}/*" = ["E501"]
`

Code(django migrations):
`
from django.db import migrations, models

class Migration(migrations.Migration):  # noqa

    dependencies = [...]
`

---

_Comment by @InSyncWithFoo on 2025-02-04 20:53_

I can't reproduce this:

```shell
$ tree
.
├── file.py
└── pyproject.toml

$ cat file.py
from django.db import migrations, models

class Migration(migrations.Migration): # noqa
    dependencies = [...]
```

```shell
$ ruff version
ruff 0.9.4

$ ruff check --select A,B,C,E,F --ignore B026,C901 .
file.py:1:35: F401 [*] `django.db.models` imported but unused
  |
1 | from django.db import migrations, models
  |                                   ^^^^^^ F401
2 |
3 | class Migration(migrations.Migration): # noqa
  |
  = help: Remove unused import: `django.db.models`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

---

_Comment by @softwaredevPK on 2025-02-04 21:53_

OMG -my bad I didn't realise that error msg were about another file....
Sorry!

---

_Closed by @softwaredevPK on 2025-02-04 21:53_

---
