```yaml
number: 2097
title: "Wrap unsafe script shebangs in `/bin/sh`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/shebang
created_at: 2024-02-29T21:45:34Z
updated_at: 2024-02-29T23:19:07Z
url: https://github.com/astral-sh/uv/pull/2097
synced_at: 2026-01-12T16:04:51Z
```

# Wrap unsafe script shebangs in `/bin/sh`

---

_@charliermarsh_

## Summary

This is based on Pradyun's installer branch (https://github.com/pradyunsg/installer/blob/d01624e5f20963f046e67d58f5319e21a07aa03e/src/installer/scripts.py#L54), which is itself based on pip (https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_vendor/distlib/scripts.py#L136).

The gist of it is: on Posix platforms, if a path contains a space (or is too long), we wrap the shebang in a `/bin/sh` invocation.

Closes https://github.com/astral-sh/uv/issues/2076.

## Test Plan

```
❯ cargo run venv "foo"
    Finished dev [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/uv venv foo`
Using Python 3.12.0 interpreter at: /Users/crmarsh/.local/share/rtx/installs/python/3.12.0/bin/python3
Creating virtualenv at: foo
Activate with: source foo/bin/activate

❯ source "foo bar/bin/activate"

❯ which black
black not found

❯ cargo run pip install black
Resolved 6 packages in 177ms
Installed 6 packages in 17ms
 + black==24.2.0
 + click==8.1.7
 + mypy-extensions==1.0.0
 + packaging==23.2
 + pathspec==0.12.1
 + platformdirs==4.2.0

❯ which black
/Users/crmarsh/workspace/uv/foo bar/bin/black

❯ black
Usage: black [OPTIONS] SRC ...

One of 'SRC' or 'code' is required.

❯ cat "foo bar/bin/black"
#!/bin/sh
'''exec' '/Users/crmarsh/workspace/uv/foo bar/bin/python' "$0" "$@"
' '''
# -*- coding: utf-8 -*-
import re
import sys
from black import patched_main
if __name__ == "__main__":
    sys.argv[0] = re.sub(r"(-script\.pyw|\.exe)?$", "", sys.argv[0])
    sys.exit(patched_main())
```

---

_Label `bug` added by @charliermarsh on 2024-02-29 21:45_

---

_Renamed from "Wrap unsafe script shebangs in /bin/sh" to "Wrap unsafe script shebangs in `/bin/sh`" by @charliermarsh on 2024-02-29 21:45_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-02-29 21:49_

---

_Marked ready for review by @charliermarsh on 2024-02-29 21:49_

---

_@charliermarsh reviewed on 2024-02-29 21:50_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:138 on 2024-02-29 21:50_

If we don't really care, we could remove the platform and OS casing. (I assume this is _safe_ to do on Windows, just not required?)

---

_Review requested from @konstin by @charliermarsh on 2024-02-29 21:56_

---

_@BurntSushi approved on 2024-02-29 22:15_

---

_@BurntSushi reviewed on 2024-02-29 22:15_

---

_Review comment by @BurntSushi on `crates/install-wheel-rs/src/wheel.rs`:138 on 2024-02-29 22:15_

I would gently nudge toward using just one value at first and see if that causes any issues. I assume shebangs longer than 127 are pretty rare?

---

_@charliermarsh reviewed on 2024-02-29 23:14_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:138 on 2024-02-29 23:14_

Done!

---

_Merged by @charliermarsh on 2024-02-29 23:19_

---

_Closed by @charliermarsh on 2024-02-29 23:19_

---

_Branch deleted on 2024-02-29 23:19_

---
