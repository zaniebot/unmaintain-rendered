```yaml
number: 11549
title: Ruff fails when attempting to produce an output file within a non-existent directory
type: issue
state: closed
author: celestialorb
labels:
  - bug
assignees: []
created_at: 2024-05-26T21:35:37Z
updated_at: 2024-05-26T23:23:13Z
url: https://github.com/astral-sh/ruff/issues/11549
synced_at: 2026-01-10T11:09:53Z
```

# Ruff fails when attempting to produce an output file within a non-existent directory

---

_Issue opened by @celestialorb on 2024-05-26 21:35_

I wasn't able to find a previously submitted issue surrounding this, apologies if I missed it.

Ruff fails (exit code `2`) when trying to write to an output file (via `--output-file`) within a directory that doesn't exist with the following message:
```
ruff failed
  Cause: No such file or directory (os error 2)
```

I'd like to see Ruff automatically create the necessary directories to house the output file if they don't already exist rather than have to ensure the directory exists before invoking Ruff; or have Ruff have an option to automatically create the directories.

Reproducing the issue is fairly simple. Setting the `--output-file` flag to a filesystem location whose directories don't exist should do the trick (e.g. `ruff check --isolated --output-file /tmp/does/not/exist/output.txt`). This was tested on the latest Ruff (`0.4.5`). Creating the necessary directories (i.e. `mkdir -p /tmp/does/not/exist`) and then rerunning the command allows Ruff to succeed.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-26 23:18_

---

_Label `bug` added by @charliermarsh on 2024-05-26 23:18_

---

_Comment by @charliermarsh on 2024-05-26 23:18_

Thanks, will fix this real quick.

---

_Closed by @charliermarsh on 2024-05-26 23:23_

---
