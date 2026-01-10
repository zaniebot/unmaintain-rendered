---
number: 9316
title: Changing the directory structure for uv init --package
type: issue
state: closed
author: benedikt-mue
labels:
  - question
assignees: []
created_at: 2024-11-21T14:19:17Z
updated_at: 2024-11-21T23:24:30Z
url: https://github.com/astral-sh/uv/issues/9316
synced_at: 2026-01-10T01:24:39Z
---

# Changing the directory structure for uv init --package

---

_Issue opened by @benedikt-mue on 2024-11-21 14:19_

Hi,
I am currently implementing uv as our package manager to replace poetry. In poetry, we defined the package entrypoints with `packages = [{ include = "backend", from = "src" }]` . 
Is there something similar possible with uv? 



---

_Comment by @charliermarsh on 2024-11-21 19:04_

I think `uv init --lib` will use a `src` layout by default.

---

_Label `question` added by @charliermarsh on 2024-11-21 19:04_

---

_Comment by @zanieb on 2024-11-21 19:10_

So does `uv init --package`:

```
❯ uv init --package example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ tree example
example
├── README.md
├── pyproject.toml
└── src
    └── example
        └── __init__.py
```

---

_Comment by @benedikt-mue on 2024-11-21 23:11_

Thanks for your quick response. I was rather curious if it is possible to change this behavior like in poetry with `[{ include = "whatever", from = "whereever" }]` ?

---

_Comment by @charliermarsh on 2024-11-21 23:12_

I'm not sure exactly what you mean, but I think you're describing something that's specific to Poetry's _build backend_.

---

_Comment by @benedikt-mue on 2024-11-21 23:16_

like changing the default layout? Say, I only want to have a flat structure:
```
example
├── README.md
├── pyproject.toml
└── project
           └── __init__.py
```

---

_Comment by @charliermarsh on 2024-11-21 23:21_

You can't change the default layout that we create, but you can certainly change your project to use that layout!

---

_Closed by @benedikt-mue on 2024-11-21 23:24_

---
