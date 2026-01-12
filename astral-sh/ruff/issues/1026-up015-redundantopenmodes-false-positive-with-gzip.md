```yaml
number: 1026
title: UP015 RedundantOpenModes false positive with gzip.open
type: issue
state: closed
author: andersk
labels: []
assignees: []
created_at: 2022-12-04T08:21:21Z
updated_at: 2022-12-04T14:12:04Z
url: https://github.com/astral-sh/ruff/issues/1026
synced_at: 2026-01-12T15:54:40Z
```

# UP015 RedundantOpenModes false positive with gzip.open

---

_@andersk_

Ruff’s fix for UP015 (#843) incorrectly changes the behavior of `gzip.open(…, "rt")`. This mode should not be flagged as redundant because the default for `gzip.open` is `"rb"`.

```console
$ cat test.py
import gzip
with gzip.open("test.gz", "rt") as f:
    print(f.read())

$ echo 'Hello, world!' | gzip > test.gz

$ python test.py
Hello, world!

$ ruff --select=UP test.py
Found 1 error(s).
test.py:2:6: UP015 Unnecessary open mode parameters
1 potentially fixable with the --fix option.

$ ruff --select=UP --fix test.py
Found 0 error(s) (1 fixed).

$ cat test.py
import gzip
with gzip.open("test.gz") as f:
    print(f.read())

$ python test.py
b'Hello, world!\n'
```

Cc @andribergs 


---

_Closed by @charliermarsh on 2022-12-04 14:12_

---
