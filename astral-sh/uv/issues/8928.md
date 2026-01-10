```yaml
number: 8928
title: uv 0.5.0 sh installer fail in github workflow
type: issue
state: closed
author: andgineer
labels: []
assignees: []
created_at: 2024-11-08T09:25:42Z
updated_at: 2024-11-08T15:07:10Z
url: https://github.com/astral-sh/uv/issues/8928
synced_at: 2026-01-10T04:36:20Z
```

# uv 0.5.0 sh installer fail in github workflow

---

_Issue opened by @andgineer on 2024-11-08 09:25_

0.5.0 version fails to install in github workflow

curl -LsSf https://astral.sh/uv/install.sh | sh

downloading uv 0.5.0 x86_64-unknown-linux-gnu
sh: 1027: _force_install_dir: parameter not set
Error: Process completed with exit code 2.

version 0.4.30 installs ok

curl -LsSf https://astral.sh/uv/0.4.30/install.sh | sh
downloading uv 0.4.30 x86_64-unknown-linux-gnu
installing to /home/runner/.cargo/bin
  uv
  uvx
everything's installed!

in both cases github runner

Ubuntu
  22.0
  LTS
  

---

_Comment by @my1e5 on 2024-11-08 11:35_

Duplicate of https://github.com/astral-sh/uv/issues/8917 and https://github.com/astral-sh/uv/issues/8919

---

_Comment by @zanieb on 2024-11-08 13:10_

Is this still occurring?

---

_Comment by @zanieb on 2024-11-08 13:11_

The CI job looks ~7hrs ago when there was a brief disruption.

---

_Closed by @zanieb on 2024-11-08 13:11_

---

_Comment by @andgineer on 2024-11-08 15:07_

 now it works

---
