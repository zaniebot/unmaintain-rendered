```yaml
number: 6595
title: Unable to run uv managed Python on an Alpine based image.
type: issue
state: closed
author: narigama
labels: []
assignees: []
created_at: 2024-08-25T02:04:38Z
updated_at: 2024-08-25T02:14:29Z
url: https://github.com/astral-sh/uv/issues/6595
synced_at: 2026-01-12T15:59:05Z
```

# Unable to run uv managed Python on an Alpine based image.

---

_@narigama_

To replicate the issue, run the following:
```bash
docker run --rm -it alpine ash

# inside the container, setup uv...
apk update
apk add --no-cache curl
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.cargo/env
uv python install 3.12

# this one fails!
uv run --verbose python --version
DEBUG uv 0.3.3
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.5-linux-x86_64-gnu`
DEBUG Failed to query Python interpreter at `/root/.local/share/uv/python/cpython-3.12.5-linux-x86_64-gnu/bin/python3`
error: Failed to query Python interpreter at `/root/.local/share/uv/python/cpython-3.12.5-linux-x86_64-gnu/bin/python3`
  Caused by: No such file or directory (os error 2)
```

---

_Comment by @narigama on 2024-08-25 02:14_

Ah, this is probably a repeat of #6392.

---

_Closed by @narigama on 2024-08-25 02:14_

---
