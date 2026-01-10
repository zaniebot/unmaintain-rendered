```yaml
number: 4079
title: commandline length limit easily reached on windows shell invocations
type: issue
state: closed
author: carpenterjc
labels:
  - bug
assignees: []
created_at: 2023-04-24T14:55:36Z
updated_at: 2023-04-25T23:58:39Z
url: https://github.com/astral-sh/ruff/issues/4079
synced_at: 2026-01-10T11:09:47Z
```

# commandline length limit easily reached on windows shell invocations

---

_Issue opened by @carpenterjc on 2023-04-24 14:55_

We use bazel to invoke ruff during the build so that the exclusions are contained in the bazel build files, and invoke ruff in a batch file which writes out a file on each batch when it is linted successfully. 

What I have noticed is that we regularly reach the command line length limit which for batch files/windows shell is very low. The normal solution of this, for things like c++ compiler/linker is to provide a response file which contains a line per argument. Something like: https://crates.io/crates/argfile for rust. However I cannot see how to do this or if it needs building into ruff. 

On windows, you will see ruff can't be invoked with a relatively small number of files if they have longish paths. 
```
import subprocess
subprocess.run([
  "ruff",
  "check",
  "--no-cache",
  "--target-version",
  "py37",
  "--config",
  "pyproject.toml",
  "--quiet",] + ["abcdefghijklmnopqrstuvwxyz/abcdefghijklmnopqrstuvwxyz/abcdefghijklmnopqrstuvwxyz/abcdefghijklmnopqrstuvwxyz.py"] * 116, 
  shell=True,
  )
```

---

_Label `bug` added by @charliermarsh on 2023-04-25 05:14_

---

_Comment by @charliermarsh on 2023-04-25 05:16_

We should be able to support this, but it does require changes within Ruff AFAICT.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-04-25 05:58_

---

_Closed by @charliermarsh on 2023-04-25 23:58_

---

_Comment by @charliermarsh on 2023-04-25 23:58_

Will be available in the next release. See #4087 for an example.

---
