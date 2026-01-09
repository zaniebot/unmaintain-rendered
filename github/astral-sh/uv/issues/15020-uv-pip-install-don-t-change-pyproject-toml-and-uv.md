---
number: 15020
title: "uv pip install don't change pyproject.toml and uv.lock. How can i sync this"
type: issue
state: closed
author: zeronerorgb
labels:
  - question
assignees: []
created_at: 2025-08-01T20:31:27Z
updated_at: 2025-08-01T21:31:33Z
url: https://github.com/astral-sh/uv/issues/15020
synced_at: 2026-01-07T13:12:19-06:00
---

# uv pip install don't change pyproject.toml and uv.lock. How can i sync this

---

_Issue opened by @zeronerorgb on 2025-08-01 20:31_

### Question

Hello. I'm did that
```
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu128
```
but it didn't change pyproject.toml and uv.lock
And i want to sync packages installed via uv pip install with pyproject.toml  and uv.lock
How can i do that.

Thank you

### Platform

Ubuntu 24.04

### Version

uv 0.6.12

---

_Label `question` added by @zeronerorgb on 2025-08-01 20:31_

---

_Comment by @zanieb on 2025-08-01 20:48_

Sounds like you want `uv add` not `uv pip`.

---

_Comment by @zanieb on 2025-08-01 20:49_

I'd also recommend reading https://docs.astral.sh/uv/guides/integration/pytorch for pytorch

---

_Comment by @zanieb on 2025-08-01 20:50_

> sync packages installed via uv pip install with pyproject.toml and uv.lock

This doesn't really exist, you should just `uv add` instead. You can simulate it with `uv pip freeze | uv add --requirements -` if you really must.

---

_Comment by @zeronerorgb on 2025-08-01 21:25_

> I'd also recommend reading https://docs.astral.sh/uv/guides/integration/pytorch for pytorch

Yes it's fix

---

_Closed by @zanieb on 2025-08-01 21:31_

---
