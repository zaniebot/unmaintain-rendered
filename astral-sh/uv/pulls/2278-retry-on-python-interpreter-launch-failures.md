```yaml
number: 2278
title: Retry on python interpreter launch failures
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/ignore-python-launch-failures
created_at: 2024-03-07T14:15:39Z
updated_at: 2024-03-07T15:07:59Z
url: https://github.com/astral-sh/uv/pull/2278
synced_at: 2026-01-10T14:54:43Z
```

# Retry on python interpreter launch failures

---

_Pull request opened by @konstin on 2024-03-07 14:15_

Sometimes, the first time we read from the stdout of the bytecode compiler python subprocess, we get an empty string back (no newline). If we try to write to stdin, it will often be a broken pipe (#2245). After we got an empty string the first time, we will get the same empty string if we read a line again.

The details of this behavior are mysterious to me, but it seems that it can be identified by the first empty string. We check by inserting starting with a `Ready` message on the Python side. When we encounter the broken state, we discard the interpreter and try again.

We have to introduce a third timeout check for the interpreter launch itself.

Minimized test script:

```bash
#!/usr/bin/env bash

set -euo pipefail

while true; do
  date --iso-8601=seconds # Progress indicator
  rm -rf testenv
  target/profiling/uv venv testenv -q --python 3.12
  VIRTUAL_ENV=$PWD/testenv target/profiling/uv pip install -q --compile wheel==0.42.0
done
```

Run as

```
cargo build --profile profiling && bash compile_bug.sh
```

Fixes #2245

---

_Label `bug` added by @konstin on 2024-03-07 14:15_

---

_@BurntSushi approved on 2024-03-07 14:44_

---

_Merged by @konstin on 2024-03-07 15:07_

---

_Closed by @konstin on 2024-03-07 15:07_

---

_Branch deleted on 2024-03-07 15:07_

---
