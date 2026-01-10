```yaml
number: 4981
title: PTH false positive with Django FieldFile
type: issue
state: closed
author: gustavi
labels:
  - question
assignees: []
created_at: 2023-06-09T08:04:48Z
updated_at: 2023-06-09T16:03:51Z
url: https://github.com/astral-sh/ruff/issues/4981
synced_at: 2026-01-10T11:09:47Z
```

# PTH false positive with Django FieldFile

---

_Issue opened by @gustavi on 2023-06-09 08:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Example of false positive for `PTH110` and `PTH116`:

```python
import os
from django.db import models
from django.test import TestCase


class Document(models.Model):
    file = models.FileField()


class MyTest(TestCase):
    def test_foo(self):
        file_path = Document.objects.last().file.path
        self.assertTrue(os.path.exists(file_path))
        self.assertEqual(os.stat(file_path).st_mode & 0o777, 0o660)

```

Output :

```
test.py:13:25: PTH110 `os.path.exists()` should be replaced by `Path.exists()`
test.py:14:26: PTH116 `os.stat()` should be replaced by `Path.stat()`, `Path.owner()`, or `Path.group()
```

`Document.objects.last().file.path` is `str`.

Tested on `ruff 0.0.272`.

Django code : https://github.com/django/django/blob/4.2/django/db/models/fields/files.py#L60

_Note: sample code can not be launched, I can provide a full working example with Django if you really need._


---

_Comment by @dhruvmanila on 2023-06-09 14:16_

I think the diagnostics are correct. What it's suggesting is the following:

```python
file_path = "foo/bar.py"

os.path.exists(file_path)

# Instead use
Path(file_path).exists()
```

Do you think it could be useful to include the variable name in the message like so:
```
test.py:13:25: PTH110 `os.path.exists(file_path)` should be replaced by `Path(file_path).exists()`
test.py:14:26: PTH116 `os.stat(file_path)` should be replaced by `Path(file_path).stat()`, `Path(file_path).owner()`, or `Path(file_path).group()
```

---

_Label `question` added by @dhruvmanila on 2023-06-09 14:16_

---

_Comment by @gustavi on 2023-06-09 15:31_

I guess my brain was not fully waked-up. Mornings... Sorry for the noise.

---

_Closed by @gustavi on 2023-06-09 15:31_

---

_Comment by @dhruvmanila on 2023-06-09 16:03_

No problem at all :)

---
