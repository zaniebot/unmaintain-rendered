---
number: 1453
title: Installing from a github zip archive extracts files with zero permissions.
type: issue
state: closed
author: bloodearnest
labels:
  - bug
assignees: []
created_at: 2024-02-16T08:38:09Z
updated_at: 2024-02-20T15:57:52Z
url: https://github.com/astral-sh/uv/issues/1453
synced_at: 2026-01-10T01:23:06Z
---

# Installing from a github zip archive extracts files with zero permissions.

---

_Issue opened by @bloodearnest on 2024-02-16 08:38_

OS: Ubuntu 22.04

I attempt to use uv to install the packages from one of our projects. We use pip-tools. In our requirements.prod.txt, we install from a git archive zip with a line like so:

```
opensafely-pipeline @ https://github.com/opensafely-core/pipeline/archive/refs/tags/v2023.11.06.145820.zip
    --hash=sha256:cc37eefd337c3cbad92bfbd7bad506d4dab9b661d66bbcf0a911eef6f5dcba56
```

uv seems to download and extracts the zip archive, but the extracted files have mode 000.

```
$ ls -l ~/.cache/uv/built-wheels-v0/url/fe42130f53efa23e/opensafely-pipeline/DwO8uSeziw_szCvcv7RS5/v2023.11.06.145820.zip/
total 59
---------- 1 wavy wavy  461 Feb 16 08:28 DEVELOPERS.md
---------- 1 wavy wavy 4584 Feb 16 08:28 justfile
---------- 1 wavy wavy  675 Feb 16 08:28 LICENSE
drwxrwxr-x 2 wavy wavy   14 Feb 16 08:28 pipeline
---------- 1 wavy wavy 1938 Feb 16 08:28 pyproject.toml
---------- 1 wavy wavy  460 Feb 16 08:28 README.md
---------- 1 wavy wavy  277 Feb 16 08:28 requirements.dev.in
---------- 1 wavy wavy 1872 Feb 16 08:28 requirements.dev.txt
---------- 1 wavy wavy  483 Feb 16 08:28 requirements.prod.txt
drwxrwxr-x 3 wavy wavy   12 Feb 16 08:28 tests
---------- 1 wavy wavy   18 Feb 16 08:28 version
```

Obviously, uv then chokes trying to read the pyproject.toml, as it doesn't have read permissions.

LMK if you need any more information.

---

_Label `bug` added by @MichaReiser on 2024-02-16 09:14_

---

_Comment by @BurntSushi on 2024-02-16 15:11_

I'm able to reproduce this, thank you!

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-16 15:16_

---

_Referenced in [astral-sh/uv#1757](../../astral-sh/uv/pulls/1757.md) on 2024-02-20 15:03_

---

_Closed by @BurntSushi on 2024-02-20 15:57_

---
