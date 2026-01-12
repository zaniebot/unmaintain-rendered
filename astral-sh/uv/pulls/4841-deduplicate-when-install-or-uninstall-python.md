```yaml
number: 4841
title: Deduplicate when install or uninstall python
type: pull_request
state: merged
author: j178
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: dedup
created_at: 2024-07-05T22:16:20Z
updated_at: 2024-07-06T05:25:15Z
url: https://github.com/astral-sh/uv/pull/4841
synced_at: 2026-01-12T16:06:29Z
```

# Deduplicate when install or uninstall python

---

_@j178_

When specifying the same argument multiple times, the same version will be downloaded multiple times:

```sh

$ cargo run -- python install --preview --force 3.12.3 cpython-3.12 3.12.3 3.12
Looking for installation Python 3.12.3 (any-3.12.3-any-any-any)
Looking for installation cpython-3.12-any-any-any (cpython-3.12-any-any-any)
Looking for installation Python 3.12.3 (any-3.12.3-any-any-any)
Looking for installation Python 3.12 (any-3.12-any-any-any)
Found 4/4 versions requiring installation
Downloading cpython-3.12.3-windows-x86_64-none
Downloading cpython-3.12.3-windows-x86_64-none
Downloading cpython-3.12.3-windows-x86_64-none
Downloading cpython-3.12.3-windows-x86_64-none
```

This PR deduplicates the `ManagedPythonDownload` before `install` or `uninstall`:

```sh
$ cargo run -q -- python install --preview --force 3.12.3 cpython-3.12 3.12.3 3.12
Looking for installation Python 3.12 (any-3.12-any-any-any)
Looking for installation Python 3.12.3 (any-3.12.3-any-any-any)
Looking for installation cpython-3.12-any-any-any (cpython-3.12-any-any-any)
Downloading cpython-3.12.3-windows-x86_64-none
Installed Python 3.12.3 to C:\Users\nigel\AppData\Roaming\uv\data\python\cpython-3.12.3-windows-x86_64-none
Installed 1 version in 6s

$ cargo run -q -- python uninstall --preview  3.12.3 cpython-3.12 3.12.3 3.12
Looking for Python installations matching Python 3.12 (any-3.12-any-any-any)
Found installation `cpython-3.12.3-windows-x86_64-none` that matches Python 3.12
Looking for Python installations matching Python 3.12.3 (any-3.12.3-any-any-any)
Looking for Python installations matching cpython-3.12-any-any-any (cpython-3.12-any-any-any)
Uninstalled `cpython-3.12.3-windows-x86_64-none`
Removed 1 Python installation
```

---

_Marked ready for review by @j178 on 2024-07-05 22:24_

---

_@charliermarsh reviewed on 2024-07-05 22:38_

IIRC dedup only deduplicates adjacent elements. Should we collect into a BTreeSet?

---

_Comment by @zanieb on 2024-07-05 23:08_

We probably need to de-duplicate the keys we're going to download too

---

_Converted to draft by @j178 on 2024-07-05 23:10_

---

_Renamed from "Dedup arguments when install or uninstall python" to "Dedupulicates when install or uninstall python" by @j178 on 2024-07-06 00:11_

---

_Renamed from "Dedupulicates when install or uninstall python" to "Deduplicates when install or uninstall python" by @j178 on 2024-07-06 00:16_

---

_Marked ready for review by @j178 on 2024-07-06 00:18_

---

_Renamed from "Deduplicates when install or uninstall python" to "Deduplicate when install or uninstall python" by @j178 on 2024-07-06 02:01_

---

_Label `bug` added by @zanieb on 2024-07-06 03:04_

---

_Label `preview` added by @zanieb on 2024-07-06 03:04_

---

_@zanieb approved on 2024-07-06 03:04_

Thank you!

---

_Merged by @zanieb on 2024-07-06 03:05_

---

_Closed by @zanieb on 2024-07-06 03:05_

---

_@charliermarsh reviewed on 2024-07-06 03:11_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/uninstall.rs`:87 on 2024-07-06 03:11_

Can we collect into a BTreeSet beforehand or no?

---

_Comment by @charliermarsh on 2024-07-06 03:11_

Thanks, your contributions are great.

---

_@zanieb reviewed on 2024-07-06 03:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/uninstall.rs`:87 on 2024-07-06 03:14_

Hm `matching_installations` is  `BTreeSet` already for deterministic ordering.

---

_Review comment by @j178 on `crates/uv/src/commands/python/uninstall.rs`:87 on 2024-07-06 05:25_

Ah, I did not realize that, I've raised #4842 to revert this.

---

_@j178 reviewed on 2024-07-06 05:25_

---

_Branch deleted on 2024-07-06 05:25_

---
