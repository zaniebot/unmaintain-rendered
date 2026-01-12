```yaml
number: 6842
title: "Failed to create fix for PytestUnittestRaisesAssertion: Unable to use existing symbol due to late binding"
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-24T07:47:30Z
updated_at: 2023-10-01T08:26:21Z
url: https://github.com/astral-sh/ruff/issues/6842
synced_at: 2026-01-12T15:54:46Z
```

# Failed to create fix for PytestUnittestRaisesAssertion: Unable to use existing symbol due to late binding

---

_@qarmin_

Ruff 0.0.285 (latest changes from main branch)

```
ruff  *.py --select ALL --no-cache
```

file content:
```
import contextlib


class C:
    def test_shared_proxy_without_callback(self):
        class MyObj:
                nonlocal obj
        with self.assertRaises(TypeError), contextlib.suppress(StopIteration):
            next(it)

from test import mapping_tests

import pytest


class WeakValueDictionaryTestCase(mapping_tests.BasicTestMappingProtocol):
        with pytest.raises(TypeError):
            weakref.finalize(a, func=fin, arg=1)
```

error:
```
Failed to create fix for PytestUnittestRaisesAssertion: Unable to use existing symbol due to late binding
```


[PY_FILE_TEST_102810345207062.py.zip](https://github.com/astral-sh/ruff/files/12426873/PY_FILE_TEST_102810345207062.py.zip)


---

_Comment by @charliermarsh on 2023-08-24 13:50_

This is expected, so we should find a way to suppress these.

---

_Label `bug` added by @charliermarsh on 2023-08-24 13:50_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-24 13:50_

---

_Comment by @qarmin on 2023-10-01 08:26_

File no longer parsable

---

_Closed by @qarmin on 2023-10-01 08:26_

---
