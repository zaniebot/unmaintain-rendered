---
number: 8715
title: uv sync --activate
type: issue
state: closed
author: dbrtly
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-10-31T02:01:52Z
updated_at: 2024-10-31T04:41:19Z
url: https://github.com/astral-sh/uv/issues/8715
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync --activate

---

_Issue opened by @dbrtly on 2024-10-31 02:01_

Sometimes it would be convenient to sync and activate a virtual environment in one command. This has probably been discussed before but I didn't find it. Apologies if so.

---

_Comment by @samypr100 on 2024-10-31 03:47_

Can you provide an example of the commands you used that didn't behave as expected? Did you try `uv sync` and it didn't work on your virtual environment?

---

_Label `question` added by @samypr100 on 2024-10-31 03:47_

---

_Comment by @dbrtly on 2024-10-31 03:51_

```
uv sync

```

Is a thing of beauty. I love it. 


I'd like the option to do 

```
uv sync --activate

```

As an alternative to 

```
uv sync
source .venv/bin/activate || source .venv/Scripts/activate

```



---

_Comment by @samypr100 on 2024-10-31 04:04_

Ah, I see. Thanks for the examples. I'd consider this a dupe of https://github.com/astral-sh/uv/issues/8106

---

_Comment by @dbrtly on 2024-10-31 04:09_

Ah. I see. 

Do you think a bash alias would work?

Like

```
alias uv-sync='uv sync && source .venv/bin/activate'

```

---

_Comment by @zanieb on 2024-10-31 04:40_

I guess so?

```
❯ alias test-example="echo 'hi' && source .venv/bin/activate"
❯ test-example
hi
❯ which python
/Users/zb/workspace/uv/.venv/bin/python
❯ deactivate
❯ test-example
hi
❯ which python
/Users/zb/workspace/uv/.venv/bin/python
```

Surprised me though tbh.

Anyway, we can't add support for this in uv directly as mentioned in the linked issue.

---

_Closed by @zanieb on 2024-10-31 04:40_

---

_Label `duplicate` added by @zanieb on 2024-10-31 04:40_

---

_Referenced in [astral-sh/uv#9166](../../astral-sh/uv/issues/9166.md) on 2024-11-16 16:03_

---
