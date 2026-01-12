```yaml
number: 1614
title: banned-api not banning module import
type: issue
state: closed
author: sbdchd
labels: []
assignees: []
created_at: 2023-01-04T02:01:37Z
updated_at: 2023-01-04T02:43:04Z
url: https://github.com/astral-sh/ruff/issues/1614
synced_at: 2026-01-12T15:54:41Z
```

# banned-api not banning module import

---

_@sbdchd_

with the config:

```toml
[tool.ruff.flake8-tidy-imports]
# Disallow all relative imports.
ban-relative-imports = "all"
[tool.ruff.flake8-tidy-imports.banned-api]
"httpx".msg = "Use kodiak.http"
```

the following code isn't banned

```python
from httpx import (
    AsyncClient,
    HTTPError,
    HTTPStatusError,
    Request,
    Response,
)
from httpx._config import DEFAULT_TIMEOUT_CONFIG
from httpx._types import TimeoutTypes
```


rel: https://github.com/charliermarsh/ruff/issues/1422#issuecomment-1370397812



---

_Comment by @charliermarsh on 2023-01-04 02:20_

You need to `select = ["TID251"]` to enable the check:

```
❯ cargo run foo/bar.py -n --select TID
    Finished dev [unoptimized + debuginfo] target(s) in 0.14s
     Running `target/debug/ruff foo/bar.py -n --select TID`
foo/bar.py:1:1: TID251 `httpx` is banned: Use kodiak.http
foo/bar.py:8:1: TID251 `httpx` is banned: Use kodiak.http
foo/bar.py:9:1: TID251 `httpx` is banned: Use kodiak.http
```

Let me know if you continue to see this behavior!

---

_Closed by @charliermarsh on 2023-01-04 02:20_

---

_Comment by @sbdchd on 2023-01-04 02:31_

Oh whoops! Thank you it works perfectly!

---

_Comment by @charliermarsh on 2023-01-04 02:43_

So happy to hear it :)

---
