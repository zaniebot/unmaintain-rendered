```yaml
number: 12962
title: failed to fetch
type: issue
state: closed
author: chenjn-n
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-04-18T05:59:24Z
updated_at: 2025-04-21T01:52:03Z
url: https://github.com/astral-sh/uv/issues/12962
synced_at: 2026-01-12T16:01:16Z
```

# failed to fetch

---

_@chenjn-n_

### Question

When I use the command
`uv pip install vllm -i https://pypi.doubanio.com/simple`
it returns an error:

![Image](https://github.com/user-attachments/assets/f8d15c82-6c97-44cd-9558-8c86dd3d4b87)

It mentions that it failed to fetch `tiktoken`,However, when I run 
`uv pip install tiktoken -i https://pypi.doubanio.com/simple`
it can successfully install `tiktoken`
![Image](https://github.com/user-attachments/assets/f82210a0-5fa7-4c3b-913f-d475d7dffb27)

Why is this happening?

### Platform

Ubuntu20.04

### Version

_No response_

---

_Label `question` added by @chenjn-n on 2025-04-18 05:59_

---

_Comment by @charliermarsh on 2025-04-18 12:16_

Sorry, I’m not sure. This happens consistently? It seems like a network issue.

---

_Closed by @charliermarsh on 2025-04-21 01:51_

---

_Label `needs-mre` added by @charliermarsh on 2025-04-21 01:52_

---
