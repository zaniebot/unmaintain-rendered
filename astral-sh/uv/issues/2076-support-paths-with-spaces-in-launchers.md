```yaml
number: 2076
title: Support paths with spaces in launchers
type: issue
state: closed
author: konstin
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-02-29T09:00:48Z
updated_at: 2024-02-29T23:19:07Z
url: https://github.com/astral-sh/uv/issues/2076
synced_at: 2026-01-12T15:58:34Z
```

# Support paths with spaces in launchers

---

_@konstin_

See https://github.com/astral-sh/uv/issues/1997#issuecomment-1970091076

When creating a new launcher, we insert the path to the python interpreter as shebang. This fails if there is a space in the path:

```python
#!/Users/ferris/my python project/.venv/bin/python
# -*- coding: utf-8 -*-
import re
import sys
from launchable import main
if __name__ == "__main__":
    sys.argv[0] = re.sub(r"(-script\.pyw|\.exe)?$", "", sys.argv[0])
    sys.exit(main())
```

The system tries to execute this as `/Users/ferris/my` with arguments `python` and `project/venv/bin/python`. We should instead use the same workaround as pip when there's a space in the path:

```python
#!/bin/sh
'''exec' "/Users/ferris/my python project/.venv/bin/python" "$0" "$@"
' '''
# -*- coding: utf-8 -*-
import re
import sys
from launchable import main
if __name__ == "__main__":
    sys.argv[0] = re.sub(r"(-script\.pyw|\.exe)?$", "", sys.argv[0])
    sys.exit(main())
```

pip reference code: https://github.com/pypa/pip/blob/0ad4c94be74cc24874c6feb5bb3c2152c398a18e/src/pip/_vendor/distlib/scripts.py#L136-L165

---

_Label `bug` added by @konstin on 2024-02-29 09:00_

---

_Label `good first issue` added by @konstin on 2024-02-29 09:00_

---

_Comment by @charliermarsh on 2024-02-29 18:58_

I can grab this one later if no one gets to it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-29 21:14_

---

_Comment by @pradyunsg on 2024-02-29 21:21_

FYI: https://github.com/pradyunsg/installer/blob/d01624e5f20963f046e67d58f5319e21a07aa03e/src/installer/scripts.py#L54


---

_Comment by @charliermarsh on 2024-02-29 21:25_

Nice, thank you.

---

_Closed by @charliermarsh on 2024-02-29 23:19_

---
