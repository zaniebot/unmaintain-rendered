---
number: 5338
title: "`uv init` should support creating a workspace"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-07-23T15:35:31Z
updated_at: 2024-08-20T20:09:58Z
url: https://github.com/astral-sh/uv/issues/5338
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv init` should support creating a workspace

---

_Issue opened by @zanieb on 2024-07-23 15:35_

I want to create a workspace layout quickly :)



---

_Label `cli` added by @zanieb on 2024-07-23 15:35_

---

_Label `preview` added by @zanieb on 2024-07-23 15:35_

---

_Comment by @j178 on 2024-07-24 00:02_

How about something like `uv init root --members foo bar/baz` to create a workspace structure like this:
```console
root
├── README.md
├── bar
│   └── baz
│       ├── README.md
│       ├── pyproject.toml
│       └── src
│           └── baz
│               └── __init__.py
├── foo
│   ├── README.md
│   ├── pyproject.toml
│   └── src
│       └── foo
│           └── __init__.py
├── pyproject.toml
└── src
    └── root
        └── __init__.py
```

We might need another flag to determine whether the root project is virtual or not.

---

_Comment by @zanieb on 2024-07-24 00:38_

Well... it might be fine to just create workspace members _after_ the first one is created. Maybe this was just blocked by one of the `init` bugs.

e.g.

```
❯ uv init
Initialized project `example`
❯ uv init ./foo
Adding `foo` as member of workspace `/Users/zb/workspace/example`
Initialized project `foo` at `/Users/zb/workspace/example/foo`
```

Seems okay? A `uv init --virtual` flag could cover the remaining use-case, right?

---

_Comment by @charliermarsh on 2024-07-24 00:50_

Yeah that all seems ok. `--virtual` could be useful.

---

_Referenced in [astral-sh/uv#5396](../../astral-sh/uv/pulls/5396.md) on 2024-07-24 04:30_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Closed by @charliermarsh on 2024-08-20 20:09_

---
