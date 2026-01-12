```yaml
number: 927
title: pip install should verify that dependencies are present
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-01-15T09:45:48Z
updated_at: 2024-01-26T02:38:00Z
url: https://github.com/astral-sh/uv/issues/927
synced_at: 2026-01-12T15:58:25Z
```

# pip install should verify that dependencies are present

---

_@konstin_

When a prior `pip install` with let's say a single package aborts after the root package is installed but before all dependencies are installed, subsequent runs will only print `Audited 1 package`, even though the venv is clearly broken. We can perform this check by reading the `METADATA` files for the root dependency and then transitively for all dependencies, i.e. fast without any network requests.

---

_Label `bug` added by @konstin on 2024-01-15 09:45_

---

_Comment by @charliermarsh on 2024-01-15 14:46_

I think if you run with `--strict` we will tell you this.

---

_Comment by @charliermarsh on 2024-01-15 14:47_

Actually, I thought we already did this for pip-install? I can look into it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-15 14:47_

---

_Unassigned @charliermarsh by @charliermarsh on 2024-01-15 14:49_

---

_Comment by @charliermarsh on 2024-01-15 14:50_

Going to need more info here since we do support this:

```
puffin on  main [$?] via 🐍 v3.12.0 (.venv) via 🦀 v1.75.0
❯ cargo run -p puffin-cli -- pip-install -r requirements.in
    Finished dev [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/puffin pip-install -r requirements.in`
Resolved 7 packages in 240ms
Downloaded 6 packages in 94ms
Installed 7 packages in 13ms
 + blinker==1.7.0
 + click==8.1.7
 + flask==3.0.0
 + itsdangerous==2.1.2
 + jinja2==3.1.3
 + markupsafe==2.1.3
 + werkzeug==3.0.1

puffin on  main [$?] via 🐍 v3.12.0 (.venv) via 🦀 v1.75.0
❯ rm -rf .venv/lib/python3.12/site-packages/click*

puffin on  main [$?] via 🐍 v3.12.0 (.venv) via 🦀 v1.75.0
❯ cargo run -p puffin-cli -- pip-install -r requirements.in
    Finished dev [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/puffin pip-install -r requirements.in`
Resolved 7 packages in 40ms
Installed 1 package in 2ms
 + click==8.1.7
```

---

_Assigned to @konstin by @charliermarsh on 2024-01-15 14:50_

---

_Comment by @charliermarsh on 2024-01-26 02:37_

Closing because I think we do this but happy to revisit.

---

_Closed by @charliermarsh on 2024-01-26 02:37_

---
