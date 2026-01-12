```yaml
number: 3110
title: Avoid enforcing EXE001 and EXE002 checks on Windows
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-02-22T03:54:37Z
updated_at: 2023-02-22T04:40:35Z
url: https://github.com/astral-sh/ruff/issues/3110
synced_at: 2026-01-12T15:54:43Z
```

# Avoid enforcing EXE001 and EXE002 checks on Windows

---

_@charliermarsh_

    I noticed this because pre-commit running on `windows-latest` [Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/hosted?view=azure-devops&tabs=yaml#software) agents, failed, where the Linux jobs pass.

    I just `pip install ruff` there ¯\\_(ツ)_/¯

_Originally posted by @basnijholt in https://github.com/charliermarsh/ruff/issues/2118#issuecomment-1439402540_


---

_Label `bug` added by @charliermarsh on 2023-02-22 03:55_

---

_Comment by @basnijholt on 2023-02-22 04:31_

Thanks for opening this and thanks for the amazing package! 

For reference, `flake8-executable` also ignore this on Windows https://github.com/xuhdev/flake8-executable/blob/2b6c3fec0f7ccb175fe34f2a613b762af2a35536/flake8_executable/__init__.py#L67-L86

---

_Closed by @charliermarsh on 2023-02-22 04:39_

---

_Comment by @charliermarsh on 2023-02-22 04:40_

Oh nice! That's good to know too! And thank you for the kind words :)

---
