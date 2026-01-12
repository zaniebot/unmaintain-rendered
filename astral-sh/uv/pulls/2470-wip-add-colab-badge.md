```yaml
number: 2470
title: "WIP: add-colab-badge"
type: pull_request
state: closed
author: raybellwaves
labels: []
assignees: []
draft: true
base: main
head: patch-1
created_at: 2024-03-15T02:16:56Z
updated_at: 2024-04-22T21:03:49Z
url: https://github.com/astral-sh/uv/pull/2470
synced_at: 2026-01-12T16:05:03Z
```

# WIP: add-colab-badge

---

_@raybellwaves_

In writing https://github.com/astral-sh/uv/issues/2469 I created this. You are more than well to copy to you own notebook so you are the owner.

https://colab.research.google.com/drive/1Lt2mw6zsqcvzmHxcqyp26EX-W5ARt1FZ#scrollTo=HNK4nQpn31BO 

nevermind below think I got it now, clunky but useable

```
!curl -LsSf https://astral.sh/uv/install.sh | sh 
import os
os.environ['PATH'] = '/root/.cargo/bin:' + os.environ['PATH']
!uv venv
!uv pip install ruff
import sys
sys.path.append(".venv/lib/python3.10/site-packages")
import ruff
```

---

_Renamed from "add-colab-badge" to "WIP: add-colab-badge" by @raybellwaves on 2024-03-15 02:45_

---

_Converted to draft by @raybellwaves on 2024-03-15 02:45_

---

_Comment by @zanieb on 2024-04-22 21:03_

Thanks for the notes! I don't think we'll prioritize this right now but I hope people find it useful.

---

_Closed by @zanieb on 2024-04-22 21:03_

---
