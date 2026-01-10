```yaml
number: 13192
title: "compression not supported: 14"
type: issue
state: closed
author: yourlogarithm
labels:
  - enhancement
assignees: []
created_at: 2025-04-29T12:25:55Z
updated_at: 2025-05-06T08:38:43Z
url: https://github.com/astral-sh/uv/issues/13192
synced_at: 2026-01-10T03:41:47Z
```

# compression not supported: 14

---

_Issue opened by @yourlogarithm on 2025-04-29 12:25_

### Summary

```sh
uv add somepackage
Resolved 58 packages in 283ms
  × Failed to download `somepackage==1.2.3`
  ├─▶ Failed to extract archive: somepackage-1.2.3-py3-none-manylinux_2_28_x86_64.whl
  ╰─▶ compression not supported: 14
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

The dependency is from a private index, but I have managed reproduce this issue by manually creating a broken `.whl` file with this script:
```python
# lzma.py

import lzma
import zipfile
import os

# Step 1: Create dummy file
os.makedirs("fakepkg", exist_ok=True)
with open("fakepkg/__init__.py", "w") as f:
    f.write("# dummy package")

# Step 2: Manually compress with LZMA
with open("fakepkg/__init__.py", "rb") as f:
    data = f.read()

compressed = lzma.compress(data)

# Step 3: Write raw ZIP with method 14 (LZMA)
from zipfile import ZipFile, ZIP_STORED, ZipInfo
import time

with ZipFile("broken-0.0.1-py3-none-any.whl", "w") as zf:
    info = ZipInfo("fakepkg/__init__.py")
    info.compress_type = 14  # LZMA
    info.date_time = time.localtime()[:6]
    info.external_attr = 0o644 << 16  # permissions
    info.file_size = len(data)
    info.compress_size = len(compressed)
    zf.writestr(info, compressed)

print("Created broken .whl with compression method 14")
```

```sh
python3 lzma.py
uv add ./broken-0.0.1-py3-none-any.whl
  × Failed to read `broken @ file:///path/broken-0.0.1-py3-none-any.whl`
  ├─▶ Failed to extract archive: broken-0.0.1-py3-none-any.whl
  ╰─▶ compression not supported: 14
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

### Platform

Fedora Linux 41 (Workstation Edition)

### Version

uv 0.6.16

### Python version

Python 3.13.3

---

_Label `bug` added by @yourlogarithm on 2025-04-29 12:25_

---

_Label `bug` removed by @konstin on 2025-04-29 12:58_

---

_Label `enhancement` added by @konstin on 2025-04-29 12:58_

---

_Comment by @konstin on 2025-04-29 13:00_

Currently, wheels are either assumed to use stored or deflate compression. Since we're already using the lzma crate for `.tar.xz` files, it's not much overhead to support it for zip files, too.

---

_Comment by @marcelbischoff on 2025-04-30 01:56_

I am getting similar errors installing uv with `pip install -U uv` in a Docker build. Pinning it to an older version fixes this so this might be a regression. 

---

_Comment by @marcelbischoff on 2025-04-30 02:11_

Nevermind runing out of disk space  in CI it looks

---

_Comment by @charliermarsh on 2025-05-04 12:49_

@konstin -- Is this hard to support?

---

_Comment by @konstin on 2025-05-04 16:02_

It has a feature flag we can activate: #13285

---

_Closed by @konstin on 2025-05-04 18:08_

---

_Comment by @j178 on 2025-05-06 08:23_

I made a wheel using LZMA compression format with this script. pip can install it, but unfortunately, I’m getting an error from uv saying “stream/file format not recognized.”

`make-lzma-wheel.py`

```python
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "wheel",
# ]
# ///
from zipfile import ZIP_LZMA

from wheel.wheelfile import WheelFile

with WheelFile("fakepkg-0.0.1-py3-none-any.whl", "w", compression=ZIP_LZMA) as zf:
    zf.writestr("fakepkg/__init__.py", b'print("hello world")')
    zf.writestr(
        "fakepkg-0.0.1.dist-info/METADATA",
        """\
Metadata-Version: 2.4
Name: fakepkg
Version: 0.0.1
Summary: fakepkg
Requires-Python: >=3.8

fakepkg is a fake package for testing purposes.
""",
    )
    zf.writestr(
        "fakepkg-0.0.1.dist-info/WHEEL",
        """\
Wheel-Version: 1.0
Generator: fakepkg
Root-Is-Purelib: false
""".encode(),
    )

print("Created fakepkg-0.0.1-py3-none-any.whl")
```

pip can install this wheel:

```console
$ uv venv --seed
$ .venv/bin/pip install fakepkg-0.0.1-py3-none-any.whl
Processing ./fakepkg-0.0.1-py3-none-any.whl
Installing collected packages: fakepkg
Successfully installed fakepkg-0.0.1
$ .venv/bin/python -c 'import fakepkg'
hello world
```

`uv add` or `uv pip install` fails:

```console
$ cargo run -- pip install fakepkg-0.0.1-py3-none-any.whl
  × Failed to read `fakepkg @ file:///private/tmp/hello/fakepkg-0.0.1-py3-none-any.whl`
  ├─▶ Failed to read metadata: `/private/tmp/hello/fakepkg-0.0.1-py3-none-any.whl`
  ├─▶ Failed to read from zip file
  ├─▶ an upstream reader returned an error: stream/file format not recognized
  ╰─▶ stream/file format not recognized

$ cargo run -- --version
uv 0.7.2+24 (3218e364a 2025-05-05)
```

---

_Comment by @konstin on 2025-05-06 08:38_

I've moved this to a separate issue: #13310

---
