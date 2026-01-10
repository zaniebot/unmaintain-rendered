```yaml
number: 2511
title: "Difference in `pip` and `uv pip` behavior with URL requirements"
type: issue
state: closed
author: gozdal
labels:
  - duplicate
assignees: []
created_at: 2024-03-18T15:29:02Z
updated_at: 2024-03-18T17:21:09Z
url: https://github.com/astral-sh/uv/issues/2511
synced_at: 2026-01-10T05:40:32Z
```

# Difference in `pip` and `uv pip` behavior with URL requirements

---

_Issue opened by @gozdal on 2024-03-18 15:29_

Say you have `pyproject.toml`:
```toml
[build-system]
requires = ["setuptools>=64"]
build-backend = "setuptools.build_meta"

[project]
requires-python = ">=3.12"
name = "foo"
version = "0.1"
dependencies = [ 
    "python-swiftclient @ git+https://github.com/StarfishStorage/python-swiftclient.git"
]
```
Create empty `src/__init__.py` and run the following script:
```bash
#!/usr/bin/env bash

set -euo pipefail

rm -rf venv wheels

uv venv --python python3.12 --seed venv
source venv/bin/activate

mkdir wheels

echo ==== WHEEL ====
python -m pip wheel --wheel-dir wheels .
python -m pip wheel --wheel-dir wheels setuptools wheel

echo ==== INSTALL ====
python -m pip install --no-index --find-links wheels foo
#uv pip install --no-index --find-links wheels foo
```

If you run `pip` in the last line it will happily install requirements
```
==== INSTALL ====
Looking in links: wheels
Processing ./wheels/foo-0.1-py3-none-any.whl
Collecting python-swiftclient@ git+https://github.com/StarfishStorage/python-swiftclient.git (from foo)
  Cloning https://github.com/StarfishStorage/python-swiftclient.git to /private/var/folders/db/7tc3n5z910s5hl292wqr4vzh0000gn/T/pip-install-otg41c5q/python-swiftclient_94c416afd2834de49ef6bc4bcc80e299
  Running command git clone --filter=blob:none --quiet https://github.com/StarfishStorage/python-swiftclient.git /private/var/folders/db/7tc3n5z910s5hl292wqr4vzh0000gn/T/pip-install-otg41c5q/python-swiftclient_94c416afd2834de49ef6bc4bcc80e299
  Resolved https://github.com/StarfishStorage/python-swiftclient.git to commit fbdac94cf75e2fb5d3cf20207d6883fc6bd280b4
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Installing backend dependencies ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: python-swiftclient
  Building wheel for python-swiftclient (pyproject.toml) ... done
  Created wheel for python-swiftclient: filename=python_swiftclient-4.3.0-py3-none-any.whl size=82568 sha256=370f823b21d217d9128fb9ac1bb72dc7c6e5b9cc9504fe84978845ae6284de16
  Stored in directory: /private/var/folders/db/7tc3n5z910s5hl292wqr4vzh0000gn/T/pip-ephem-wheel-cache-d0kikjfc/wheels/ec/86/0a/94b581e62da6d0db5ab11231273afb01f3953d639dd5bfb651
Successfully built python-swiftclient
Installing collected packages: python-swiftclient, foo
Successfully installed foo-0.1 python-swiftclient-4.3.0
```
but `uv` will fail with an error:
```
==== INSTALL ====
error: Package `python-swiftclient` attempted to resolve via URL: git+https://github.com/StarfishStorage/python-swiftclient.git. URL dependencies must be expressed as direct requirements or constraints. Consider adding `python-swiftclient @ git+https://github.com/StarfishStorage/python-swiftclient.git` to your dependencies or constraints file.
```

Not sure which behavior is "correct".
* `pip` will not use `python_swiftclient-4.3.0-py3-none-any.whl` generated in the `wheels` directory, it will build the wheel again
* `uv` will refuse to install `python-swiftclient` giving somewhat misleading error (the dependency is specified in the format provided).

`uv 0.1.21`, `Python 3.12.2`

---

_Comment by @zanieb on 2024-03-18 15:48_

Thanks for the well written issue!

This is a duplicate of https://github.com/astral-sh/uv/issues/1808 and is covered in detail in our [pip-compatibility guide](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#transitive-direct-url-dependencies).

---

_Label `duplicate` added by @zanieb on 2024-03-18 15:49_

---

_Comment by @gozdal on 2024-03-18 15:57_

Thanks for the quick reply. I wonder if it's the same issue though - documentation says about transitive URL dependencies. I don't understand why it affects direct dependency - my package  `foo` in the example depends on `python-swiftclient @ git+https://github.com/StarfishStorage/python-swiftclient.git`, so this shouldn't be a transitive dependency?

---

_Comment by @zanieb on 2024-03-18 16:06_

You're installing `foo` in your uv invocation not `python-swiftclient`, so it's transitive not a directly requested requirement.

If you were to run `uv pip install -e .` this would not be the case, since the project and its listed dependencies are your direct requirements.

---

_Comment by @gozdal on 2024-03-18 16:09_

Thanks for the explanation. The error message mislead me, asking to add a requirement which I already have.

---

_Closed by @charliermarsh on 2024-03-18 17:21_

---
