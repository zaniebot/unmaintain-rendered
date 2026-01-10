```yaml
number: 1308
title: Document that Python 3.12.0 is required to run benchmarks
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
assignees: []
merged: true
base: main
head: benchmark-doc-python-3.12.0
created_at: 2024-02-15T07:37:16Z
updated_at: 2024-02-15T14:54:53Z
url: https://github.com/astral-sh/uv/pull/1308
synced_at: 2026-01-10T15:33:24Z
```

# Document that Python 3.12.0 is required to run benchmarks

---

_Pull request opened by @MichaReiser on 2024-02-15 07:37_

Update the Benchmark documentation to explicitly document that it requires Python 3.12.0 to run the non puffin benchmarks:

```
Benchmark 2: poetry (resolve-warm)

Current Python version (3.12.1) is not allowed by the project (3.12).
Please change python executable via the "env use" command.
Error: Command terminated with non-zero exit code: 1. Use the '-i'/'--ignore-failure' option if you want to ignore this. Alternatively, use the '--show-output' option to debug what went wrong.
```


---

_Review requested from @charliermarsh by @MichaReiser on 2024-02-15 07:37_

---

_Label `documentation` added by @MichaReiser on 2024-02-15 07:37_

---

_@MichaReiser reviewed on 2024-02-15 07:37_

---

_Review comment by @MichaReiser on `BENCHMARKS.md`:86 on 2024-02-15 07:37_

I'm fairly certain that this should be `install-warm`

---

_Merged by @charliermarsh on 2024-02-15 14:54_

---

_Closed by @charliermarsh on 2024-02-15 14:54_

---

_Branch deleted on 2024-02-15 14:54_

---

_Comment by @charliermarsh on 2024-02-15 14:54_

Thanks!

---
