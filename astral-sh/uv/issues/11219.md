```yaml
number: 11219
title: Project virtual environment creation is not concurrency-safe
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-02-04T16:28:44Z
updated_at: 2025-02-05T20:19:58Z
url: https://github.com/astral-sh/uv/issues/11219
synced_at: 2026-01-10T03:50:31Z
```

# Project virtual environment creation is not concurrency-safe

---

_Issue opened by @charliermarsh on 2025-02-04 16:28_

E.g., this script fails if you run it repeatedly:

```sh
#!/bin/bash

rm -rf uvtest
mkdir -p uvtest
cd uvtest
uv init
rm -rf uvtest/.venv

run_python() {
  uv run python -c "print('Hello from process $1')"
}

for i in {1..5}; do
  run_python $i &
done

wait
# Return to original directory
cd - > /dev/null
```

With some combination of:

```shell
❯ ./sync.sh
Initialized project `uvtest`
Using CPython 3.13.0
Creating virtual environment at: .venv
error: Project virtual environment directory `/Users/crmarsh/workspace/puffin/uvtest/.venv` cannot be used because it is not a valid Python environment (no Python executable was found)
error: Project virtual environment directory `/Users/crmarsh/workspace/puffin/uvtest/.venv` cannot be used because it is not a valid Python environment (no Python executable was found)
Using CPython Using CPython 3.13.03.13.0

error: Project virtual environment directory `/Users/crmarsh/workspace/puffin/uvtest/.venv` cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment
error: Project virtual environment directory `/Users/crmarsh/workspace/puffin/uvtest/.venv` cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment
Hello from process 4
```

---

_Label `bug` added by @charliermarsh on 2025-02-04 16:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-04 17:06_

---

_Comment by @charliermarsh on 2025-02-05 03:14_

For context... the `uv pip` interface, we do place an advisory lock on the environment, to avoid concurrent modification.

---

_Comment by @zanieb on 2025-02-05 05:55_

Presumably part of the problem is that we're attempting to use the environment when it's partially constructed. With `uv pip`, we can lock the environment with a file inside it. Here, the directory may not exist? and if it's invalid we may delete it anyway. It seems a little annoying to write a lock to the project root directory.

---

_Comment by @charliermarsh on 2025-02-05 13:40_

There’s no reason that the lock has to be in any specific location. We could put it in the cache even.

---

_Comment by @charliermarsh on 2025-02-05 14:36_

I'll give it some thought. I do think this should work, though.

---

_Comment by @zanieb on 2025-02-05 15:05_

I thought co-location was important for some reason, but.. I can't think of a reason now.

---

_Comment by @charliermarsh on 2025-02-05 15:07_

Also somewhat wondering if we can just make `virtualenv` creation robust to this somehow. I'll take a look.

---

_Closed by @charliermarsh on 2025-02-05 20:19_

---

_Closed by @charliermarsh on 2025-02-05 20:19_

---
