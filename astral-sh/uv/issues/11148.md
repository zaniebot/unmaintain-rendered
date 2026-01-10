```yaml
number: 11148
title: "Add \"last updated\" date to documentation"
type: issue
state: closed
author: zanieb
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-01-31T22:47:57Z
updated_at: 2025-02-04T03:28:49Z
url: https://github.com/astral-sh/uv/issues/11148
synced_at: 2026-01-10T03:50:31Z
```

# Add "last updated" date to documentation

---

_Issue opened by @zanieb on 2025-01-31 22:47_

Ref https://github.com/squidfunk/mkdocs-material/discussions/5301

Google picks up the `exclude-newer` timestamp which is back in 2006?

---

_Label `documentation` added by @zanieb on 2025-01-31 22:48_

---

_Label `help wanted` added by @zanieb on 2025-01-31 22:48_

---

_Comment by @LEvinson2504 on 2025-02-02 09:57_

Hi, I would like to pick this issue, I added the mentioned dependency, and mkdocs serve locally, it looks good to me and is picking the last commit from git perfectly. I have run the first of following commands to update the docs/requirements.txt, the second I'm not sure if I am not privileged to run but its taking forever on my end. let me know if I should be able to run it, will give it time and push the update of docs/insiders_requirements.txt aswell.

1. uv pip compile docs/requirements.in -o docs/requirements.txt --universal -p 3.12 # ran
2. uv pip compile docs/requirements-insiders.in -o docs/requirements-insiders.txt --universal -p 3.12 # stuck

---

_Closed by @charliermarsh on 2025-02-04 03:28_

---

_Closed by @charliermarsh on 2025-02-04 03:28_

---
