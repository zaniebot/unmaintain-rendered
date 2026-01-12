```yaml
number: 12655
title: UV lock upgrade not upgrading anything
type: issue
state: closed
author: RafaelCenzano
labels:
  - question
assignees: []
created_at: 2025-04-03T15:15:35Z
updated_at: 2025-04-03T16:51:27Z
url: https://github.com/astral-sh/uv/issues/12655
synced_at: 2026-01-12T16:01:09Z
```

# UV lock upgrade not upgrading anything

---

_@RafaelCenzano_

### Question

I wanted to ask if this is expected because I expect packages to be able to upgrade like in other uv projects I have but I'm confused why upgrade is not working, if it's an issue with my current versions (just convert to uv on this project) or if there's some other issue I'm not seeing.

Pyproject.toml https://github.com/alpha-phi-omega-ez/photo-discord-bot/blob/main/pyproject.toml
uv.lock https://github.com/alpha-phi-omega-ez/photo-discord-bot/blob/main/uv.lock

I attached the verbose output from the uv lock --upgrade command, it doesn't fail it runs through no problem it just doesn't upgrade anything.

[temp.txt](https://github.com/user-attachments/files/19589896/temp.txt)

### Platform

macOS 15.4 ARM (Darwin 24.4.0 arm64)

### Version

uv 0.6.12 (Homebrew 2025-04-02)

---

_Label `question` added by @RafaelCenzano on 2025-04-03 15:15_

---

_Comment by @zanieb on 2025-04-03 16:39_

Your `pyproject.toml` pins dependencies

https://github.com/alpha-phi-omega-ez/photo-discord-bot/blob/28c4b58c0ced4ea6c6f8f94882f05b03cbd022fd/pyproject.toml#L7-L44

so they cannot be upgraded in the `uv.lock` file. 

Instead, you should have constraints, e.g., `>=1.0` in the `pyproject.toml` and rely on the `uv.lock` file to pin dependencies to specific versions.

---

_Comment by @RafaelCenzano on 2025-04-03 16:51_

Thanks that makes a lot of sense not sure how I missed that

---

_Closed by @RafaelCenzano on 2025-04-03 16:51_

---
