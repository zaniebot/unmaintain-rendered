```yaml
number: 1452
title: Git dependencies fetch strategy
type: issue
state: closed
author: TheoBabilon
labels:
  - bug
assignees: []
created_at: 2024-02-16T08:27:10Z
updated_at: 2024-02-21T18:44:33Z
url: https://github.com/astral-sh/uv/issues/1452
synced_at: 2026-01-10T05:40:31Z
```

# Git dependencies fetch strategy

---

_Issue opened by @TheoBabilon on 2024-02-16 08:27_

Hi team,

Thanks for the great work on `uv`!

I have several git dependencies in one of my existing projects; I tried running 
`uv pip compile pyproject.toml -o requirements.txt`  but it fails with

```bash
Updating ssh://git (github.mycompany.com/toto/abcd.git)                               

error: Failed to download and build: abcd @ git+ssh://git@github.mycompany.com/toto/abcd.git
  Caused by: Git operation failed
  Caused by: failed to fetch into: /myhomedir/.cache/uv/git-v0/db/f81ed0a494f1ba1c
  Caused by: failed to authenticate when downloading repository

* attempted ssh-agent authentication, but no usernames succeeded: `myusername`, `git`
  Caused by: no authentication methods succeeded
  ```

Note that cloning repos works fine from git cli (or even using `pdm`, though I'm not sure what's the underlying method used). 

Looking into https://github.com/astral-sh/uv/pull/283, I noticed from @charliermarsh comment that this is unfortunately a common problem with ssh deps with Cargo; 
looking into the PR it seems that the Cli fetching strategy is handled (which might potentially solve my problem?), but it is unclear to me if this is somehow exposed to us (as end user), through the `uv` cli?



---

_Label `bug` added by @MichaReiser on 2024-02-16 08:52_

---

_Comment by @TudorAndrei on 2024-02-16 10:02_

I'm encountering the same issue

---

_Closed by @zanieb on 2024-02-21 18:44_

---
