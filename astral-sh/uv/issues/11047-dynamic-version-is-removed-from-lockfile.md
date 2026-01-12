```yaml
number: 11047
title: Dynamic version is removed from lockfile occasionally
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2025-01-29T01:12:32Z
updated_at: 2025-01-29T01:27:10Z
url: https://github.com/astral-sh/uv/issues/11047
synced_at: 2026-01-12T16:00:26Z
```

# Dynamic version is removed from lockfile occasionally

---

_@charliermarsh_

> @charliermarsh Use this repo: https://github.com/apollo13/uv-bug
> 
> Follow this to the letter (I am not sure if you can repro if you do anything different):
> 
>  * `uv cache clean`
>  * `uv lock` and observe that `git status` shows no changes
>  * `uv sync`
>  * `rm -rf uv.lock .venv`
>  * `uv lock`
>  * Run `git diff` and see the version written into the lock file (shouldn't be)
>  * `uv cache clean` 
>  * `rm -rf uv.lock .venv`
>  * `uv lock` (No version written -> okay) 

 _Originally posted by @apollo13 in [#7533](https://github.com/astral-sh/uv/issues/7533#issuecomment-2620117459)_

---

_Label `bug` added by @charliermarsh on 2025-01-29 01:12_

---

_Closed by @charliermarsh on 2025-01-29 01:27_

---

_Closed by @charliermarsh on 2025-01-29 01:27_

---
