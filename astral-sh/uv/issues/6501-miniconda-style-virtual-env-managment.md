```yaml
number: 6501
title: Miniconda style virtual env managment
type: issue
state: closed
author: sash-a
labels:
  - duplicate
assignees: []
created_at: 2024-08-23T09:07:45Z
updated_at: 2024-08-23T13:18:47Z
url: https://github.com/astral-sh/uv/issues/6501
synced_at: 2026-01-12T15:59:04Z
```

# Miniconda style virtual env managment

---

_@sash-a_

Hi there, thanks for this tool, it's been a pleasure to use!

I have a feature request that I think would be useful. In conda you can have globally accessible envs and activate them anywhere with `conda activate env_name`. These envs simply live in `~/.miniconda`. I really like this feature because I often find myself keeping an old venv while working on a feature that requires a new dependency and creating a new venv for that. I think it would be nice to be able to run `uv activate env_name` from anywhere instead of `source path/to/project/.venv/bin/activate`. This would likely require an option when creating a venv, e.g `-n <env_name>` which would then automatically store the env somewhere sensible under that name and allow for `uv activate env_name`

This definitely may be out of scope for uv, so feel free to ignore it :smile: 

---

_Comment by @zanieb on 2024-08-23 13:18_

Hi! I think this is a duplicate of https://github.com/astral-sh/uv/issues/1495

---

_Closed by @zanieb on 2024-08-23 13:18_

---

_Label `duplicate` added by @zanieb on 2024-08-23 13:18_

---
