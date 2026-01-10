```yaml
number: 1304
title: Ty does not work with uvx --with
type: issue
state: closed
author: MeGaGiGaGon
labels: []
assignees: []
created_at: 2025-10-04T00:20:24Z
updated_at: 2025-10-04T07:22:14Z
url: https://github.com/astral-sh/ty/issues/1304
synced_at: 2026-01-10T02:06:25Z
```

# Ty does not work with uvx --with

---

_Issue opened by @MeGaGiGaGon on 2025-10-04 00:20_

### Summary

Not sure if this is fixable on ty's end, but trying to type check code using modules from `--with` does not work. It would be nice if it did for quick testing/runs.
Example:
```powershell
PS ~\python_projects>echo "from typing import reveal_type;import pytest;reveal_type(pytest.fail)" > issue.py
PS ~\python_projects>uvx --with pytest ty check issue.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                         
error[unresolved-import]: Cannot resolve imported module `pytest`
 --> issue.py:1:39
  |
1 | from typing import reveal_type;import pytest;reveal_type(pytest.fail)
  |                                       ^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. C:\...\python_projects (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. C:\...\python_projects\.venv\Lib\site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

info[revealed-type]: Revealed type
 --> issue.py:1:58
  |
1 | from typing import reveal_type;import pytest;reveal_type(pytest.fail)
  |                                                          ^^^^^^^^^^^ `Unknown`
  |

Found 2 diagnostics
```
Also note that `~\python_projects` does not have a `.venv`

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Closed by @MichaReiser on 2025-10-04 07:22_

---
