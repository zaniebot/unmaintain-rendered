```yaml
number: 9206
title: "Better error message when `git` is not found"
type: pull_request
state: merged
author: blueraft
labels:
  - error messages
assignees: []
merged: true
base: main
head: improve-error-message-git-not-found
created_at: 2024-11-18T16:47:36Z
updated_at: 2024-11-18T17:41:22Z
url: https://github.com/astral-sh/uv/pull/9206
synced_at: 2026-01-12T16:08:42Z
```

# Better error message when `git` is not found

---

_@blueraft_

## Summary

Closes https://github.com/astral-sh/uv/issues/9200


## Test Plan

Using the following Dockerfile:
```Dockerfile
FROM debian:latest

RUN apt-get update && apt-get install -y python3

WORKDIR /app
COPY target/debug/uv .
RUN chmod +x uv

RUN /app/uv venv && /app/uv pip install git@github.com:pallets/flask.git
```

```
❯ cargo build -q -p uv && docker build .
...
 => ERROR [6/6] RUN /app/uv venv && /app/uv pip install git@github.com:pallets/flask.git                    0.4s
------
 > [6/6] RUN /app/uv venv && /app/uv pip install git@github.com:pallets/flask.git:
0.275 Using CPython 3.11.2 interpreter at: /usr/bin/python3
0.275 Creating virtual environment at: .venv
0.318   × Failed to download and build `git @
0.318   │ file:///app/github.com:pallets/flask.git`
0.318   ├─▶ Git operation failed
0.318   ╰─▶ Git executable not found. Ensure that Git is installed and available.
------
Dockerfile:9
--------------------
   7 |     RUN chmod +x uv
   8 |
   9 | >>> RUN /app/uv venv && /app/uv pip install git@github.com:pallets/flask.git
  10 |
--------------------
ERROR: failed to solve: process "/bin/sh -c /
```




---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-18 17:15_

---

_Label `error messages` added by @charliermarsh on 2024-11-18 17:15_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-11-18 17:15_

---

_@charliermarsh reviewed on 2024-11-18 17:22_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:30 on 2024-11-18 17:22_

I think we should make this `#[error(transparent)]` and pass the source unmodified.

---

_@blueraft reviewed on 2024-11-18 17:27_

---

_Review comment by @blueraft on `crates/uv-git/src/git.rs`:30 on 2024-11-18 17:27_

Done

---

_@charliermarsh approved on 2024-11-18 17:41_

---

_Merged by @charliermarsh on 2024-11-18 17:41_

---

_Closed by @charliermarsh on 2024-11-18 17:41_

---
