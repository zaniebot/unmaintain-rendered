```yaml
number: 10237
title: "How to install ruff on debian if it is not packaged, and pip3 complains about \"virtual environments\"?"
type: issue
state: closed
author: ardabro
labels:
  - question
assignees: []
created_at: 2024-03-05T13:47:01Z
updated_at: 2024-03-29T20:28:38Z
url: https://github.com/astral-sh/ruff/issues/10237
synced_at: 2026-01-12T15:54:50Z
```

# How to install ruff on debian if it is not packaged, and pip3 complains about "virtual environments"?

---

_@ardabro_

Ruff is not packaged in Debian stable repo and I'm unable to install it without creating a virtual environment, which I don't want to do, because - I suppose - in such env, I'll lose access to all other system-wide packages I use.
So: how can I use it in Debian?

    > pip3 install --user ruff
    error: externally-managed-environment
    
    × This environment is externally managed
    ╰─> To install Python packages system-wide, try apt install
        python3-xyz, where xyz is the package you are trying to
        install.

It is the same with and without `--user` switch

---

_Comment by @charliermarsh on 2024-03-05 13:55_

Have you considered using `pipx` (https://github.com/pypa/pipx)? That would be my recommendation.

---

_Label `question` added by @charliermarsh on 2024-03-05 13:55_

---

_Renamed from "How to install ruff on debian if it is not packaged, and pip3 complains about "virtual environaments"?" to "How to install ruff on debian if it is not packaged, and pip3 complains about "virtual environments"?" by @AlexWaygood on 2024-03-05 14:52_

---

_Comment by @ardabro on 2024-03-05 16:37_

> Have you considered using `pipx` (https://github.com/pypa/pipx)? That would be my recommendation.

I see... Not sure if it is going to work when I want to run it inside sublime-text plugin pylsp 

---

_Comment by @charliermarsh on 2024-03-05 16:41_

Can you share more on why this wouldn't work? In either case, Ruff will end up in your `PATH`, right?

---

_Comment by @charliermarsh on 2024-03-29 20:28_

Ah, you need to run with `--break-system-packages` if you want to install into the system Python without a virtual environment on Debian.

---

_Closed by @charliermarsh on 2024-03-29 20:28_

---
