```yaml
number: 2350
title: Initialize client lazily for remote requirements files
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/lazy
created_at: 2024-03-11T00:29:07Z
updated_at: 2024-03-11T00:42:40Z
url: https://github.com/astral-sh/uv/pull/2350
synced_at: 2026-01-10T14:49:08Z
```

# Initialize client lazily for remote requirements files

---

_Pull request opened by @charliermarsh on 2024-03-11 00:29_

## Summary

We now initialize an HTTP client in advance for remote requirements files. It turns out this adds a significant overhead, even for operations like auditing the environment (at least on macOS).

This PR makes initialization lazy. After a lot of evaluation, I took the easiest route, which is: we just pass in `Connectivity`, and then use the default HTTP client. So we won't respect netrc files and anything else that we get from our registry client. If we want to keep using the registry client, we _can_, it's just way more ceremony to pass down a closure.

See: https://github.com/astral-sh/uv/issues/2346.

## Test Plan

- Verified that `cargo run pip compile https://raw.githubusercontent.com/ansible/ansible/f1ded0f41759235eb15a7d13dbc3c95dce5d5acd/requirements.txt` completed without error.
- Verified that `cargo run pip compile https://raw.githubusercontent.com/ansible/ansible/f1ded0f41759235eb15a7d13dbc3c95dce5d5acd/requirements.txt --offline` failed with an error.
- Verified that `./target/release/uv pip install requests` completed in 0-2ms, rather than hundreds.


---

_Label `performance` added by @charliermarsh on 2024-03-11 00:29_

---

_Comment by @charliermarsh on 2024-03-11 00:34_

I'll look at re-adding the middleware in a future PR.

---

_Merged by @charliermarsh on 2024-03-11 00:42_

---

_Closed by @charliermarsh on 2024-03-11 00:42_

---

_Branch deleted on 2024-03-11 00:42_

---
