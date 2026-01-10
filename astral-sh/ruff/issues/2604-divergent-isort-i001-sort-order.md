```yaml
number: 2604
title: Divergent isort I001 sort order
type: issue
state: closed
author: moritz89
labels: []
assignees: []
created_at: 2023-02-06T10:46:14Z
updated_at: 2023-02-06T15:33:37Z
url: https://github.com/astral-sh/ruff/issues/2604
synced_at: 2026-01-10T11:09:45Z
```

# Divergent isort I001 sort order

---

_Issue opened by @moritz89 on 2023-02-06 10:46_

Ruff reports an error with the following code even though the imports were sorted with `isort`.

```py
# test.py
from json import decoder
from json import encoder as j_encoder
from json import tool

jdec = decoder.JSONDecoder()
jenv = j_encoder.JSONEncoder()
tool.main()
```

```bash
ruff test.py --select I --isolated

test.py:1:1: I001 Import block is un-sorted or un-formatted
Found 1 error.
1 potentially fixable with the --fix option.

ruff --version
ruff 0.0.239
```

The order that ruff expects instead is:

```py
# test.py
from json import decoder, tool
from json import encoder as j_encoder

jdec = decoder.JSONDecoder()
jenv = j_encoder.JSONEncoder()
tool.main()
```


---

_Comment by @ngnpope on 2023-02-06 11:17_

This is a [documented](https://github.com/charliermarsh/ruff#how-does-ruffs-import-sorting-compare-to-isort) divergence from `isort`, notably the link to https://github.com/charliermarsh/ruff/issues/1381.

---

_Comment by @charliermarsh on 2023-02-06 15:33_

Yeah just closing as a dupe. Appreciate the clear issue.

---

_Closed by @charliermarsh on 2023-02-06 15:33_

---
