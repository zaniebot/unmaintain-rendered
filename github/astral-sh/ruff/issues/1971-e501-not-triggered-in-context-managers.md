---
number: 1971
title: E501 not triggered in context managers
type: issue
state: closed
author: zevisert
labels:
  - bug
assignees: []
created_at: 2023-01-18T19:29:33Z
updated_at: 2023-01-18T21:25:26Z
url: https://github.com/astral-sh/ruff/issues/1971
synced_at: 2026-01-07T13:12:14-06:00
---

# E501 not triggered in context managers

---

_Issue opened by @zevisert on 2023-01-18 19:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Example code:
```py
# ruff-example.py

with open(
    file="aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
):
    pass

file = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"

```

Command: 

```console
> ruff --isolated ruff-example.py
ruff-example.py:8:89: E501 Line too long (180 > 88 characters)
Found 1 error(s).
```

Expected:
- Two E501 errors: One on line 4 and as well as the reported one on line 8.

Version: 
```console
> ruff -V                               
ruff 0.0.224
```

---

_Comment by @charliermarsh on 2023-01-18 19:33_

I think we're probably allowing that right now because it looks like "one long token", since there's no space delimiting it within the line.

It's similar to how we allow:
```py
"""aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"""
```

Since there's no reasonable way for the user to break up the string.

---

_Comment by @charliermarsh on 2023-01-18 19:34_

Probably hard to fix right now because that check isn't AST- or token-aware.

---

_Label `bug` added by @charliermarsh on 2023-01-18 19:34_

---

_Comment by @zevisert on 2023-01-18 20:16_

Ah - I think you're correct on that. 

The actual example I ran into looked more like this: 

```py
# ruff-example.py
import os

from httpx import Client

API_V4_URL = os.environ["API_V4_URL"]
PROJECT_ID = os.environ["PROJECT_ID"]
LOCATION_IID = os.environ["LOCATION_IID"]
RESOURCE_IID = os.environ["RESOURCE_IID"]
API_TOKEN = os.environ["API_TOKEN"]

with Client(
    base_url=f"{API_V4_URL}/projects/{PROJECT_ID}/locations/{LOCATION_IID}/resources/{RESOURCE_IID}/updates",
    headers={"Authorization": f"Bearer {API_TOKEN}"},
) as client:
    pass
```

The f-string must act as a single token. If I refactor to use a join instead, I'll start seeing F501 again.

```py
with Client(
    base_url="/".join([API_V4_URL, "projects", PROJECT_ID, "locations", LOCATION_IID, "resources", RESOURCE_IID, "updates"]),
    headers={"Authorization": f"Bearer {API_TOKEN}"},                                 # ^ ruff-example.py:18:89: E501 Line too long (98 > 88 characters)
) as client:
    pass
```

Feel free to close this, I think this behaviour is fine as is. 

---

_Comment by @charliermarsh on 2023-01-18 21:25_

Ahh ok -- thank you for following up!

---

_Closed by @charliermarsh on 2023-01-18 21:25_

---
