```yaml
number: 7308
title: uv pip install h2o stuck
type: issue
state: closed
author: thunder-007
labels:
  - question
  - needs-mre
assignees: []
created_at: 2024-09-11T20:25:51Z
updated_at: 2024-09-16T02:46:59Z
url: https://github.com/astral-sh/uv/issues/7308
synced_at: 2026-01-12T15:59:12Z
```

# uv pip install h2o stuck

---

_@thunder-007_

uv pip install h2o
```
username@host:~/path$ uv pip install h2o
⠴ h2o==3.46.0.5                                                                                                                                                                         
⠧ h2o==3.46.0.5                                                                                                                                                                         
⠙ h2o==3.46.0.5                                                                                                                                                                         
⠹ h2o==3.46.0.5                                                                                                                                                                         
⠸ h2o==3.46.0.5                                                                                                                                                                         
⠸ h2o==3.46.0.5
^C
```
after some enters no progress so exited , but it's working fine with pip.

---

_Comment by @ibraheemdev on 2024-09-11 20:37_

What terminal/environment are you running in? The `--no-progress` flag should help avoid the broken output, it succeeds in around 20 seconds on my machine.

---

_Label `question` added by @zanieb on 2024-09-11 20:50_

---

_Comment by @thunder-007 on 2024-09-12 08:03_

kubuntu 24.04 kde konsole let me try the --no-progress tag


---

_Comment by @thunder-007 on 2024-09-12 08:09_

it's working but I don't see the progress like I do for other libraries.

---

_Comment by @charliermarsh on 2024-09-16 02:46_

I don't know if there's much we can do here. Closing but can reopen if we have an MRE.

---

_Closed by @charliermarsh on 2024-09-16 02:46_

---

_Label `needs-mre` added by @charliermarsh on 2024-09-16 02:46_

---
