```yaml
number: 7011
title: "`uv` shouldn't modify `pyproject.toml` on failed install"
type: issue
state: closed
author: kdheepak
labels:
  - bug
assignees: []
created_at: 2024-09-04T12:41:47Z
updated_at: 2024-09-04T14:42:16Z
url: https://github.com/astral-sh/uv/issues/7011
synced_at: 2026-01-12T15:59:09Z
```

# `uv` shouldn't modify `pyproject.toml` on failed install

---

_@kdheepak_

I ran the following:

```
$ uv add dsfkdsjfldskfjds
error: Request failed after 3 retries
  Caused by: error sending request for url (https://pypi.org/simple/dsfkdsjfldskfjds/)
  Caused by: client error (Connect)
  Caused by: invalid peer certificate: UnknownIssuer
````

But that resulted in a change to `pyproject.toml`:

<img width="867" alt="image" src="https://github.com/user-attachments/assets/8cec1411-f244-4e53-a89e-92cd2e635273">



---

_Comment by @kdheepak on 2024-09-04 12:49_

This seems to happen only if there's a certificate failure. After setting `export UV_NATIVE_TLS=true`, and running the same thing, I get a different error:

<img width="870" alt="image" src="https://github.com/user-attachments/assets/60753b64-7472-4f8d-8cfb-31c878a4a2a9">

But this time `dependencies` is not updated in `pyproject.toml`.

---

_Comment by @charliermarsh on 2024-09-04 13:21_

Interesting, thanks.

---

_Label `bug` added by @charliermarsh on 2024-09-04 13:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-04 13:22_

---

_Comment by @zanieb on 2024-09-04 13:25_

Ah I encountered this too recently.

---

_Closed by @charliermarsh on 2024-09-04 14:42_

---
