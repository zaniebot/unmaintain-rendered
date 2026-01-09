---
number: 6427
title: Package upgrading
type: issue
state: closed
author: jacoverster
labels:
  - question
assignees: []
created_at: 2024-08-22T12:27:56Z
updated_at: 2024-08-22T23:21:33Z
url: https://github.com/astral-sh/uv/issues/6427
synced_at: 2026-01-07T13:12:17-06:00
---

# Package upgrading

---

_Issue opened by @jacoverster on 2024-08-22 12:27_

uv sync --upgrade not responding as expected. I expect it to upgrade the package to the latest version but it keeps it at the added version.

To reproduce:
```
uv init
uv add requests==2.0.0
uv sync --upgrade
uv tree
```

Output:
![image](https://github.com/user-attachments/assets/e48618ac-fc0b-4ca3-8e97-d7eb04cdc2b6)

Debug output:
![image](https://github.com/user-attachments/assets/2e7cf405-e7d7-4165-82de-9db27b850438)

My desired result can be achieved using:
```
uv remove requests
uv add requests
```
![image](https://github.com/user-attachments/assets/6f8629fe-03cf-4ada-999c-b4004f89794e)


Using uv 0.3.1



---

_Comment by @charliermarsh on 2024-08-22 12:39_

If you use `requests==2.0.0`, then we add `requests==2.0.0` to your dependencies. `uv lock --upgrade` won't change any dependency _definitions_, it will just upgrade the lockfile to the latest compatible versions. But given `requests==2.0.0`, `2.0.0` is the latest compatible version you could ever get (since, e.g., `2.0.1` does not equal `2.0.0`).

I'd suggest `uv add "requests>=2.0.0"` or similar. Does that make sense?


---

_Label `question` added by @charliermarsh on 2024-08-22 12:39_

---

_Comment by @jacoverster on 2024-08-22 12:53_

Thanks for the clarification, that makes 100% sense.

---

_Closed by @charliermarsh on 2024-08-22 23:21_

---

_Comment by @charliermarsh on 2024-08-22 23:21_

No prob!

---
