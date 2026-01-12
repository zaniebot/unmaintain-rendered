```yaml
number: 4169
title: "[request] uv pip install -r pyproject.toml[optional_deps]"
type: issue
state: closed
author: gmankab
labels:
  - question
assignees: []
created_at: 2024-06-09T02:28:34Z
updated_at: 2024-06-10T01:43:00Z
url: https://github.com/astral-sh/uv/issues/4169
synced_at: 2026-01-12T15:58:48Z
```

# [request] uv pip install -r pyproject.toml[optional_deps]

---

_@gmankab_

### feature request

i really like `uv pip install -r pyproject.toml` feature, which installs project dependencis but does not installs project iteslf

also there is a support for optional dependences installing with `uv pip install .[optional_deps]`

can you please provide optional dependencies installing when `-r` flag is used, so then i can install project optional dependencies, but not install project itself

idk which syntax would be better for it, maybe something like `uv pip install -r pyproject.toml[optional_deps]`

### why

when i debelop projects i use `uv pip install -r pyproject.toml` to install dependencies for my project

also there is autotests dependences, which only needed to run autotests, and should not be installed on computers of people who don't contribute to the project and don't running autotests

so i want to use uv to install testing dependences to the venv


---

_Comment by @zanieb on 2024-06-09 14:25_

Hi it sounds like you're looking for the `--extra` flag

```
      --extra <EXTRA>
          Include optional dependencies in the given extra group name; may be provided more than once
```



---

_Label `question` added by @zanieb on 2024-06-09 14:25_

---

_Closed by @charliermarsh on 2024-06-10 01:43_

---
