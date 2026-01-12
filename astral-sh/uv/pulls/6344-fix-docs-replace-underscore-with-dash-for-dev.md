```yaml
number: 6344
title: "fix(docs): replace underscore with dash for dev-dependencies"
type: pull_request
state: merged
author: ldacey
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-08-21T15:51:55Z
updated_at: 2024-08-21T16:22:15Z
url: https://github.com/astral-sh/uv/pull/6344
synced_at: 2026-01-12T16:07:19Z
```

# fix(docs): replace underscore with dash for dev-dependencies

---

_@ldacey_

Docs show an underscore which should be a dash in dev-dependencies: `dev_dependencies = ["ruff==0.5.0"]`


## Summary
I followed the example in the references settings and used dev_dependencies in my pyproject.toml but it seems like this needs to be a dash instead of an underscore:

 => ERROR [stage-0 5/5] RUN uv sync                                                                                                                                                                                                                                   6.9s
------
 > [stage-0 5/5] RUN uv sync:
0.085 warning: Failed to parse `pyproject.toml` during settings discovery:
0.085   TOML parse error at line 65, column 1
0.085      |
0.085   65 | [tool.uv]
0.085      | ^^^^^^^^^
0.085   unknown field `dev_dependencies`
0.085



---

_@charliermarsh approved on 2024-08-21 15:52_

Thank you!

---

_Label `documentation` added by @charliermarsh on 2024-08-21 15:52_

---

_Merged by @charliermarsh on 2024-08-21 15:52_

---

_Closed by @charliermarsh on 2024-08-21 15:52_

---

_Comment by @zanieb on 2024-08-21 16:08_

@charliermarsh this is reference documentation — needs to be replaced in the code. Don't know how this passed generation checks.

---

_Branch deleted on 2024-08-21 16:08_

---

_Comment by @charliermarsh on 2024-08-21 16:09_

Oh oops. I'll do it. I actually _did_ check for that but I just misread.

---

_Comment by @charliermarsh on 2024-08-21 16:11_

(And verify why CI passed.)

---

_Comment by @charliermarsh on 2024-08-21 16:22_

Oh, hah, it's because we don't run tests if you _only_ change Markdown.

---
