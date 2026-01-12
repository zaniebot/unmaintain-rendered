```yaml
number: 11970
title: "Index URLs aren't deduplicated when fetching"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - performance
assignees: []
created_at: 2025-03-05T02:47:37Z
updated_at: 2025-05-02T08:29:08Z
url: https://github.com/astral-sh/uv/issues/11970
synced_at: 2026-01-12T16:00:51Z
```

# Index URLs aren't deduplicated when fetching

---

_@charliermarsh_

If you run `uv pip install flask --index https://pypi.org/simple --index-strategy unsafe-first-match`, we fetch from PyPI twice: once for the default index, and again for `--index https://pypi.org/simple `.

---

_Label `bug` added by @charliermarsh on 2025-03-05 02:47_

---

_Label `performance` added by @charliermarsh on 2025-03-05 02:47_

---

_Comment by @christeefy on 2025-04-28 15:21_

Hi @charliermarsh, I'm interested in working on this, and I've verified that this bug still exists on `main` today:
```
...
DEBUG Found stale response for: https://pypi.org/simple/flask/
DEBUG Sending revalidation request for: https://pypi.org/simple/flask/
DEBUG starting new connection: https://pypi.org/
DEBUG Found stale response for: https://pypi.org/simple/flask/
DEBUG Sending revalidation request for: https://pypi.org/simple/flask/
DEBUG starting new connection: https://pypi.org/
...
```

I like to confirm — is this still relevant and that there's no duplicate ongoing work before I start on this?

Thanks in advance!

---

_Closed by @konstin on 2025-05-02 08:29_

---
