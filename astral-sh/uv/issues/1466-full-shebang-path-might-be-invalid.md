```yaml
number: 1466
title: Full shebang path might be invalid
type: issue
state: closed
author: astrojuanlu
labels:
  - bug
assignees: []
created_at: 2024-02-16T09:56:47Z
updated_at: 2024-03-04T01:51:04Z
url: https://github.com/astral-sh/uv/issues/1466
synced_at: 2026-01-12T15:58:28Z
```

# Full shebang path might be invalid

---

_@astrojuanlu_

I noticed that `uv` creates executables with the full interpreter path in the shebang. Which is a smart solution, but might create invalid scripts:

```
> pwd
/Users/juan_cano/Projects/QuantumBlack Labs/tmp/test-project-uv1
> which python
/Users/juan_cano/Projects/QuantumBlack Labs/tmp/test-project-uv1/.venv/bin/python
> uv pip install mypy
Resolved 4 packages in 4.77s
Downloaded 3 packages in 1.10s
Installed 3 packages in 27ms
 + mypy==1.8.0
 + mypy-extensions==1.0.0
 + typing-extensions==4.9.0
> mypy --help
exec: Failed to execute process '/Users/juan_cano/Projects/QuantumBlack Labs/tmp/test-project-uv1/.venv/bin/mypy': The file exists and is executable. Check the interpreter or linker?
> head (which mypy)
#!/Users/juan_cano/Projects/QuantumBlack Labs/tmp/test-project-uv1/.venv/bin/python
# -*- coding: utf-8 -*-
import re
import sys
from mypy.__main__ import console_entry
if __name__ == "__main__":
    sys.argv[0] = re.sub(r"(-script\.pyw|\.exe)?$", "", sys.argv[0])
    sys.exit(console_entry())
> "/Users/juan_cano/Projects/QuantumBlack Labs/tmp/test-project-uv1/.venv/bin/python" (which mypy) -V
mypy 1.8.0 (compiled: yes)
```

This is similar to https://github.com/conda/conda/issues/186 

---

_Label `bug` added by @MichaReiser on 2024-02-16 11:57_

---

_Comment by @charliermarsh on 2024-02-16 14:48_

Thanks!

---

_Comment by @charliermarsh on 2024-03-04 01:51_

I believe this was fixed by https://github.com/astral-sh/uv/pull/2097.

---

_Closed by @charliermarsh on 2024-03-04 01:51_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-04 01:51_

---
