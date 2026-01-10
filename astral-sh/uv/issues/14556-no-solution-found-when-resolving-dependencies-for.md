---
number: 14556
title: "No solution found when resolving dependencies for split (python_full_version == '3.9.*')"
type: issue
state: open
author: xidianwang412
labels:
  - question
assignees: []
created_at: 2025-07-11T03:45:14Z
updated_at: 2025-07-12T12:14:51Z
url: https://github.com/astral-sh/uv/issues/14556
synced_at: 2026-01-10T01:25:46Z
---

# No solution found when resolving dependencies for split (python_full_version == '3.9.*')

---

_Issue opened by @xidianwang412 on 2025-07-11 03:45_

### Summary

执行：uv add "mcp[cli]"
输出：
No solution found when resolving dependencies for split (python_full_version == '3.9.*'):
  ╰─▶ Because the requested Python version (>=3.9) does not satisfy Python>=3.10 and your project depends on mcp[cli], we can conclude that your project's requirements are
      unsatisfiable.

      hint: While the active Python version is 3.10, the resolution failed for other Python versions supported by your project. Consider limiting your project's supported
      Python versions using `requires-python`.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.

前置操作：
conda activate python=3.10.18虚拟环境
在此环境下依次执行
 un init projectxxx
uv venv --python 3.10
 source .venv/bin/activate



### Platform

macos

### Version

uv 0.7.20 (251420396 2025-07-09)

### Python version

python 3.10.18

---

_Label `bug` added by @xidianwang412 on 2025-07-11 03:45_

---

_Comment by @charliermarsh on 2025-07-12 12:04_

Do you have `requires-python = ">=3.9"` in your `pyproject.toml`? You need at least `requires-python = ">=3.10"`. The error message is telling you that `mcp[cli]` doesn't support Python 3.9, so uv can't resolve for that version, and you need to raise the minimum supported version in your project.

---

_Label `bug` removed by @charliermarsh on 2025-07-12 12:04_

---

_Label `question` added by @charliermarsh on 2025-07-12 12:04_

---

_Comment by @charliermarsh on 2025-07-12 12:06_

This is the error message I get:

```
  × No solution found when resolving dependencies for split (python_full_version == '3.9.*'):
  ╰─▶ Because the requested Python version (>=3.9) does not satisfy Python>=3.10 and your project depends on mcp[cli],
      we can conclude that your project's requirements are unsatisfiable.

      hint: While the active Python version is 3.13, the resolution failed for other Python versions supported by your
      project. Consider limiting your project's supported Python versions using `requires-python`.
```

I feel like we should try to improve the error message to make clear that `mcp[cli]` needs `Python>=3.10`. (\cc @zanieb)

Edit: although confusingly, the error message in this other issue https://github.com/astral-sh/uv/issues/14549 does include "{dependency} depends on Python", maybe it's getting simplified out somewhere.

---

_Comment by @charliermarsh on 2025-07-12 12:14_

```
Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on projectxxx*
  term projectxxx*
    term projectxxx==0.1.0
      projectxxx==0.1.0 depends on mcp[cli]*
      term mcp[cli]*
        no versions of Python>=3.10
        no versions of mcp[cli]<0.9.1 | >0.9.1, <1.0.0 | >1.0.0, <1.1.0 | >1.1.0, <1.1.1 | >1.1.1, <1.1.2 | >1.1.2, <1.1.3 | >1.1.3, <1.2.0 | >1.2.0, <1.2.1 | >1.2.1, <1.3.0 | >1.3.0, <1.4.0 | >1.4.0, <1.4.1 | >1.4.1, <1.5.0 | >1.5.0, <1.6.0 | >1.6.0, <1.7.0 | >1.7.0, <1.7.1 | >1.7.1, <1.8.0 | >1.8.0, <1.8.1 | >1.8.1, <1.9.0 | >1.9.0, <1.9.1 | >1.9.1, <1.9.2 | >1.9.2, <1.9.3 | >1.9.3, <1.9.4 | >1.9.4, <1.10.0 | >1.10.0, <1.10.1 | >1.10.1, <1.11.0 | >1.11.0
    no versions of projectxxx<0.1.0 | >0.1.0
Resolver derivation tree after reduction
term projectxxx==0.1.0
  projectxxx==0.1.0 depends on mcp[cli]*
  no versions of Python>=3.10
```

---
