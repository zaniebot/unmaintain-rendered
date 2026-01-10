```yaml
number: 4452
title: "`--no-compile` flag recommends `--no_compile`"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
created_at: 2024-06-23T17:16:15Z
updated_at: 2024-06-23T17:25:51Z
url: https://github.com/astral-sh/uv/issues/4452
synced_at: 2026-01-10T05:31:37Z
```

# `--no-compile` flag recommends `--no_compile`

---

_Issue opened by @charliermarsh on 2024-06-23 17:16_

@charliermarsh  I'd like to reopen this issue, since --no-compile flag is still not support. Getting this error:
```
$ uv pip install --no-compile test
error: unexpected argument '--no-compile' found

  tip: a similar argument exists: '--no_compile'
```
which kind of misses the point (tested on uv==0.2.13)

_Originally posted by @staaam in https://github.com/astral-sh/uv/issues/2771#issuecomment-2185156424_
            

---

_Label `bug` added by @charliermarsh on 2024-06-23 17:16_

---

_Label `cli` added by @charliermarsh on 2024-06-23 17:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-23 17:16_

---

_Closed by @charliermarsh on 2024-06-23 17:25_

---

_Closed by @charliermarsh on 2024-06-23 17:25_

---
