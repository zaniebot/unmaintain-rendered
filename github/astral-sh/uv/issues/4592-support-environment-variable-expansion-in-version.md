---
number: 4592
title: Support Environment Variable Expansion in Version Specifiers
type: issue
state: open
author: akan72
labels:
  - enhancement
assignees: []
created_at: 2024-06-27T18:02:07Z
updated_at: 2024-06-28T12:09:27Z
url: https://github.com/astral-sh/uv/issues/4592
synced_at: 2026-01-07T13:12:17-06:00
---

# Support Environment Variable Expansion in Version Specifiers

---

_Issue opened by @akan72 on 2024-06-27 18:02_

Specifying the versions of related packages via envvar would be really useful for us! 

Here's an example of what we used to be able to do via `pip-tools`:
```
dagster==${DAGSTER_VERSION}
dagster-cloud==${DAGSTER_VERSION}
dagster-cloud-cli==${DAGSTER_VERSION}
```

> The originating issue here (supporting environment variables in `-r` and `-c` paths, within requirements files) is now fixed by https://github.com/astral-sh/uv/pull/2143. If folks want environment variable expansion for other parts of the file (like version specifiers), we need to handle those on a case-by-case basis, so feel free to open a new issue to track it.

_Originally posted by @charliermarsh in https://github.com/astral-sh/uv/issues/1473#issuecomment-1975402403_
            

---

_Comment by @charliermarsh on 2024-06-27 18:03_

We could probably support this, but I don't think we would preserve the environment variables in the output file (unlike for URLs, where we go to great lengths to support it).

---

_Label `enhancement` added by @charliermarsh on 2024-06-28 12:09_

---

_Referenced in [astral-sh/uv#5016](../../astral-sh/uv/issues/5016.md) on 2024-07-12 16:10_

---
