```yaml
number: 6543
title: "Set `VIRTUAL_ENV` for `uv run` invocations"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/ephemeral
created_at: 2024-08-23T19:48:46Z
updated_at: 2024-08-23T20:03:39Z
url: https://github.com/astral-sh/uv/pull/6543
synced_at: 2026-01-10T13:09:51Z
```

# Set `VIRTUAL_ENV` for `uv run` invocations

---

_Pull request opened by @zanieb on 2024-08-23 19:48_

If we don't do this, and `uv run` invokes something like `uv run --isolated uv pip install foo` uv won't mutate the isolated environment, it'll mutate whatever outer environment it finds.

---

_Label `bug` added by @zanieb on 2024-08-23 19:48_

---

_Comment by @zanieb on 2024-08-23 19:52_

Doesn't work..

```
❯ cargo run -- run --isolated -v uv pip install httpx -v
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.16s
     Running `target/debug/uv run --isolated -v uv pip install httpx -v`
DEBUG uv 0.3.2
DEBUG Found project root: `/Users/zb/workspace/uv`
DEBUG Project `uv` is marked as unmanaged
DEBUG No project found; searching for Python interpreter
DEBUG Reading requests from `/Users/zb/workspace/uv/.python-version`
DEBUG Searching for Python 3.11 in managed installations or system path
WARN Broken interpreter cache entry at /Users/zb/.cache/uv/interpreter-v2/363ce54fc41c903a.msgpack, removing: wrong msgpack marker FixStr(28)
DEBUG Found `cpython-3.12.4-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Searching for managed installations at `/Users/zb/Library/Application Support/uv/python`
DEBUG Found managed installation `cpython-3.11.7-macos-aarch64-none`
DEBUG Found `cpython-3.11.7-macos-aarch64-none` at `/Users/zb/Library/Application Support/uv/python/cpython-3.11.7-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Creating isolated virtual environment
INFO Ignoring empty directory
DEBUG Using Python 3.11.7 interpreter at: /Users/zb/.cache/uv/builds-v0/.tmpY3q4fk/bin/python
DEBUG Running `uv pip install httpx -v`
DEBUG uv 0.3.0
DEBUG Searching for Python interpreter in system path
WARN Broken interpreter cache entry at /Users/zb/.cache/uv/interpreter-v2/363ce54fc41c903a.msgpack, removing: invalid type: boolean `true`, expected path string
DEBUG Found `cpython-3.12.4-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.4 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG Requirement satisfied: anyio
DEBUG Requirement satisfied: certifi
DEBUG Requirement satisfied: h11<0.15, >=0.13
DEBUG Requirement satisfied: httpcore==1.*
DEBUG Requirement satisfied: httpx
DEBUG Requirement satisfied: idna
DEBUG Requirement satisfied: idna>=2.8
DEBUG Requirement satisfied: sniffio
DEBUG Requirement satisfied: sniffio>=1.1
Audited 1 package in 24ms
```

---

_Renamed from "Set `VIRTUAL_ENV` for `uv run` invocations in ephemeral environments" to "Set `VIRTUAL_ENV` for `uv run` invocations" by @zanieb on 2024-08-23 19:56_

---

_Comment by @zanieb on 2024-08-23 19:56_

Test with `cargo run -- run --isolated -v uv pip install httpx -v`

---

_Review requested from @charliermarsh by @zanieb on 2024-08-23 19:57_

---

_@charliermarsh approved on 2024-08-23 19:58_

---

_Merged by @zanieb on 2024-08-23 20:03_

---

_Closed by @zanieb on 2024-08-23 20:03_

---

_Branch deleted on 2024-08-23 20:03_

---
