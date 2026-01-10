```yaml
number: 14470
title: shebang rewrite in installed scripts do not respect extra args passed to the interpreter
type: issue
state: closed
author: mayeut
labels:
  - bug
  - help wanted
  - compatibility
assignees: []
created_at: 2025-07-06T10:27:41Z
updated_at: 2025-07-09T15:51:08Z
url: https://github.com/astral-sh/uv/issues/14470
synced_at: 2026-01-10T03:32:45Z
```

# shebang rewrite in installed scripts do not respect extra args passed to the interpreter

---

_Issue opened by @mayeut on 2025-07-06 10:27_

### Summary

e.g. a script in the wheel looks like:
```
#!python -E
import sys


from cmake import cpack

if __name__ == '__main__':
    if sys.argv[0].endswith('-script.pyw'):
        sys.argv[0] = sys.argv[0][: -11]
    elif sys.argv[0].endswith('.exe'):
        sys.argv[0] = sys.argv[0][: -4]

    sys.exit(cpack())
```

The script gets installed as:
```
#!/bin/sh
'''exec' '/private/var/folders/hb/2n_7f3yn20v06lzz3129dtw80000gn/T/cibw-run-sz3abzyq/cp39-macosx_universal2/venv-test-x86_64/bin/python3' "$0" "$@"
' ''' -E

import sys


from cmake import cpack

if __name__ == '__main__':
    if sys.argv[0].endswith('-script.pyw'):
        sys.argv[0] = sys.argv[0][: -11]
    elif sys.argv[0].endswith('.exe'):
        sys.argv[0] = sys.argv[0][: -4]

    sys.exit(cpack())
```

The `-E` argument is not being written where it should be.
pip installing the same wheel creates a working script.
The [spec](https://packaging.python.org/en/latest/specifications/binary-distribution-format/#recommended-installer-features) is unclear but the assumption in pip (or maybe distutils) is that arguments present behind the interpreter shall be left untouched (i.e. not removed).


### Platform

macOS 15

### Version

uv 0.7.19

### Python version

Python 3.12.10

---

_Label `bug` added by @mayeut on 2025-07-06 10:27_

---

_Renamed from "shebangs in installed scripts do not respect extra args passed to the interpreter" to "shebang rewrite in installed scripts do not respect extra args passed to the interpreter" by @mayeut on 2025-07-06 10:28_

---

_Label `compatibility` added by @zanieb on 2025-07-06 22:39_

---

_Comment by @charliermarsh on 2025-07-07 14:49_

Seems reasonable to preserve if we can.

---

_Label `help wanted` added by @charliermarsh on 2025-07-07 14:49_

---

_Comment by @charliermarsh on 2025-07-09 14:08_

I just used `pip install` for a wheel containing this script:

```python
#!python -E
import sys
print("Python executable:", sys.executable)
print("Arguments:", sys.argv)
print("Environment isolated:", "-E" in sys.flags)
```

Here's the executable it put in my virtual environment:

```python
#!/Users/crmarsh/workspace/uv/.venv/bin/python
import sys
print("Python executable:", sys.executable)
print("Arguments:", sys.argv)
print("Environment isolated:", "-E" in sys.flags)
```

Your comment suggests that pip retains these, but it looks like it doesn't?

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-09 14:11_

---

_Comment by @charliermarsh on 2025-07-09 14:43_

I've confirmed from the code that pip drops them, so going to do the same here.

---

_Closed by @zanieb on 2025-07-09 15:51_

---
