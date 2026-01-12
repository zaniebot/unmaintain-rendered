```yaml
number: 6845
title: Bad inline suggestion for requirements.txt
type: issue
state: closed
author: dimaqq
labels:
  - error messages
assignees: []
created_at: 2024-08-30T00:37:31Z
updated_at: 2024-10-10T22:09:35Z
url: https://github.com/astral-sh/uv/issues/6845
synced_at: 2026-01-12T15:59:08Z
```

# Bad inline suggestion for requirements.txt

---

_@dimaqq_

So I didn't read `--help` / usage, and tried this:

```command
> uvx --with requirements.txt tox --help
? `requirements.txt` looks like a local requirements file but was passed as a package name. Did you mean `-r requirements.txt`? [y/n] › yes
```

However, neither `uvx -r req...` nor `uvx --with -r req...` do what's intended.

Instead, the correct flag is `uvx --with-requirements req...`


---

_Label `error messages` added by @charliermarsh on 2024-08-30 12:46_

---

_Assigned to @charliermarsh by @zanieb on 2024-09-03 13:53_

---

_Closed by @charliermarsh on 2024-10-10 22:09_

---

_Closed by @charliermarsh on 2024-10-10 22:09_

---
