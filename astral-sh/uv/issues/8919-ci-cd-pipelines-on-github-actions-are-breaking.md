---
number: 8919
title: CI/CD pipelines on Github Actions are breaking
type: issue
state: closed
author: vfilter
labels: []
assignees: []
created_at: 2024-11-08T05:23:11Z
updated_at: 2024-11-08T05:50:00Z
url: https://github.com/astral-sh/uv/issues/8919
synced_at: 2026-01-10T01:24:34Z
---

# CI/CD pipelines on Github Actions are breaking

---

_Issue opened by @vfilter on 2024-11-08 05:23_

All our pipelines on Github Actions just started breaking. We've been installing uv until now like this:
```yaml
      - name: Set up uv
        run: curl -LsSf https://astral.sh/uv/install.sh | sh
```

Tried this but it also breaks:
```yaml
      - name: Set up uv
        run: curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.5.0/uv-installer.sh | sh
```

Error is this:
![image](https://github.com/user-attachments/assets/849a8779-9db8-4701-866d-079552718e07)


---

_Comment by @fraser-langton on 2024-11-08 05:28_

same as #8917

`curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.4.30/uv-installer.sh | sh` works, but downgrading also has breaking changes to PATH 🥺


---

_Comment by @zanieb on 2024-11-08 05:42_

It should be fixed now. Please let me know if not.

---

_Closed by @zanieb on 2024-11-08 05:50_

---

_Referenced in [astral-sh/uv#8928](../../astral-sh/uv/issues/8928.md) on 2024-11-08 11:35_

---
