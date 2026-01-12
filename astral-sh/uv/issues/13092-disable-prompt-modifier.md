```yaml
number: 13092
title: Disable prompt modifier?
type: issue
state: closed
author: norweeg
labels:
  - question
assignees: []
created_at: 2025-04-24T15:24:02Z
updated_at: 2025-04-25T03:08:06Z
url: https://github.com/astral-sh/uv/issues/13092
synced_at: 2026-01-12T16:01:19Z
```

# Disable prompt modifier?

---

_@norweeg_

### Question

I use oh-my-zsh and oh-my-posh (depending on OS and Shell I am working in).  These tools already modify the command prompt to include information about the active python environment; therefore, I do not want or need the uv-generated venv activation script to modify it at all.   I have tried using `--prompt ""` in hopes that specifying a modifier of an empty string would effectively disable it, but it is treated as if I didn't pass the `--prompt` flag at all and a default prompt modifier is used.  How do I disable/turn off the prompt modifier when creating a venv?

### Platform

Windows 11 x86_64, Ubuntu 24.04 x86_64, Ubuntu 24.04 aarch64

### Version

uv 0.6.16

---

_Label `question` added by @norweeg on 2025-04-24 15:24_

---

_Comment by @norweeg on 2025-04-25 03:08_

nevermind.  Figured it out myself.  you just set environment variable `VIRTUAL_ENV_DISABLE_PROMPT=1`.  I wish there was a note in the documentation of the `--prompt` flag documentation that made note of this.   

---

_Closed by @norweeg on 2025-04-25 03:08_

---
