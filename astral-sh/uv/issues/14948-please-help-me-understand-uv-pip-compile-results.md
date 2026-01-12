```yaml
number: 14948
title: Please help me understand uv pip compile results
type: issue
state: closed
author: skyanth
labels:
  - question
assignees: []
created_at: 2025-07-29T05:51:55Z
updated_at: 2025-07-30T08:15:15Z
url: https://github.com/astral-sh/uv/issues/14948
synced_at: 2026-01-12T16:02:00Z
```

# Please help me understand uv pip compile results

---

_@skyanth_

### Question

I have 2 projects. They are both on my system, both in (their own) .venv with Python 3.13.5. uv is 0.7.20 for both. Both have the same dependecies in their pyproject.toml:

```
dependencies = [
    "brng-config>=1.0.1",
    "wb-core-brng>=3.3.15",
]
```

... actually, pyproject.toml is a copy, the projects are copies, they are the same minimal backend importing a core package and running two different frontends.

Their uv.lock files are identical.

But when I do `uv pip compile pyproject.toml -o requirements.txt --universal`, I get different versions on a whole list of packages:

```limits: 5.4.0 vs 4.2
h11: 0.16.0 vs 0.14.0
propcache: 0.3.2 vs 0.3.0
cryptography: 45.0.5 vs 44.0.2
certifi: 2025.7.14 vs 2025.1.31
packaging: 25.0 vs 24.2
lxml: 6.0.0 vs 5.3.1
charset-normalizer: 3.4.2 vs 3.4.1
redis: 6.2.0 vs 5.2.1
pypika-tortoise: 0.6.1 vs 0.5.0
yarl: 1.20.1 vs 1.18.3
requests: 2.32.4 vs 2.32.3
uvicorn: 0.35.0 vs 0.34.0
pytz: 2025.2 vs 2025.1
click: 8.2.1 vs 8.1.8
typing-extensions: 4.14.1 vs 4.12.2
anyio: 4.9.0 vs 4.8.0
urllib3: 2.5.0 vs 2.3.0
tortoise-orm: 0.25.1 vs 0.24.2
pillow: 11.3.0 vs 11.1.0
tomlkit: 0.13.3 vs 0.13.2
multidict: 6.6.3 vs 6.1.0
fastapi: 0.116.1 vs 0.115.11
starlette: 0.47.2 vs 0.46.1
setuptools: 80.9.0 vs 76.0.0
aerich: 0.9.1 vs 0.8.2
httpcore: 1.0.9 vs 1.0.7
```

Note that these are all dependencies of my wb-core-brng 3.3.15 package, which I also created myself on this system, on the same day.

EDIT: I should add that I re-created their .venvs from scratch and deleted all python caches and egg folders as well.

Can someone explain to me how this works?

### Platform

MacOS 15.5

### Version

uv 0.7.20 (Homebrew 2025-07-09)

---

_Label `question` added by @skyanth on 2025-07-29 05:51_

---

_Comment by @zanieb on 2025-07-29 12:22_

Can you share a full reproduction? It sounds like there's more going on.

---

_Comment by @skyanth on 2025-07-29 12:32_

Hey, a full reproduction of what exactly? The requirements.txt files? I would also surmise there's more going on, I was hoping someone would be able to think of a factor I was overlooking.

---

_Comment by @zanieb on 2025-07-29 12:58_

A full reproduction being a set of commands and files that I can use to reproduce the problem. I don't have enough information to help otherwise.

See https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/

---

_Comment by @skyanth on 2025-07-30 08:15_

Hi, I did an upgrade of uv and a machine reboot in the meantime and the issue disappeared, so nevermind. Thanks for getting back to me anyway. :)

---

_Closed by @skyanth on 2025-07-30 08:15_

---
