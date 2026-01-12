```yaml
number: 669
title: Add explicit error message for URLs without package names
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/url-error
created_at: 2023-12-16T21:11:01Z
updated_at: 2023-12-16T21:14:35Z
url: https://github.com/astral-sh/uv/pull/669
synced_at: 2026-01-12T16:04:06Z
```

# Add explicit error message for URLs without package names

---

_@charliermarsh_

`pip` supports installing packages without names (e.g., `git+https://github.com/pallets/flask.git`), but it doesn't adhere to the PEP grammar, and we don't yet support it (and may never) (#313).

This PR adds a dedicated error path for such cases, to ensure that we can give meaningful feedback to the user:

```
error: Couldn't parse requirement in requirements.in position 0 to 18
  Caused by: URL requirement is missing a package name; expected: `package_name @ https://google.com`
https://google.com
^^^^^^^^^^^^^^^^^^
```

Closes https://github.com/astral-sh/puffin/issues/650.

---

_Label `error messages` added by @charliermarsh on 2023-12-16 21:11_

---

_Merged by @charliermarsh on 2023-12-16 21:14_

---

_Closed by @charliermarsh on 2023-12-16 21:14_

---

_Branch deleted on 2023-12-16 21:14_

---
