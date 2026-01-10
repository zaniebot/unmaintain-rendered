```yaml
number: 2805
title: Support freethreading python
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/gil-disabled-abiflags
created_at: 2024-04-03T13:27:59Z
updated_at: 2024-04-12T09:39:49Z
url: https://github.com/astral-sh/uv/pull/2805
synced_at: 2026-01-10T14:43:31Z
```

# Support freethreading python

---

_Pull request opened by @konstin on 2024-04-03 13:27_

freethreaded python reintroduces abiflags since it is incompatible with regular native modules and abi3.

Tests: None yet! We're lacking cpython 3.13 no-gil builds we can use in ci.

My test setup:

```
PYTHON_CONFIGURE_OPTS="--enable-shared --disable-gil" pyenv install 3.13.0a5
cargo run -q -- venv -q -p python3.13 .venv3.13 --no-cache-dir && cargo run -q -- pip install -v psutil --no-cache-dir && .venv3.13/bin/python -c "import psutil"
```

Fixes #2429

---

_Renamed from "Support no-gil python" to "Support freethreaded python" by @konstin on 2024-04-03 14:00_

---

_Renamed from "Support freethreaded python" to "Support freethreading python" by @konstin on 2024-04-03 14:04_

---

_Label `enhancement` added by @zanieb on 2024-04-03 14:49_

---

_@zanieb approved on 2024-04-03 14:49_

Cool

---

_Comment by @henryiii on 2024-04-03 20:46_

deadsnakes has free threading builds, FYI. Haven't started using them in CI yet, but have seen it here: https://github.com/deadsnakes/action.

---

_Marked ready for review by @konstin on 2024-04-12 09:27_

---

_Comment by @konstin on 2024-04-12 09:27_

I'll add a test case when we have a good test candidate, psutil currently just errors with a different message

---

_Merged by @konstin on 2024-04-12 09:39_

---

_Closed by @konstin on 2024-04-12 09:39_

---

_Branch deleted on 2024-04-12 09:39_

---
