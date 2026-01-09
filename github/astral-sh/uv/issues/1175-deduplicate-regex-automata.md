---
number: 1175
title: "Deduplicate `regex-automata`"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2024-01-29T21:00:56Z
updated_at: 2025-08-30T01:08:12Z
url: https://github.com/astral-sh/uv/issues/1175
synced_at: 2026-01-07T13:12:16-06:00
---

# Deduplicate `regex-automata`

---

_Issue opened by @charliermarsh on 2024-01-29 21:00_

It looks like `tracing-subscriber` is pulling in an old version:

```text
❯ cargo tree -p puffin -i regex-automata@0.1.10
regex-automata v0.1.10
└── matchers v0.1.0
    └── tracing-subscriber v0.3.18
        ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin)
        └── tracing-tree v0.3.0
            └── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin)
```

Whereas elsewhere, we use `regex-automata@0.4.4`.

---

_Label `internal` added by @charliermarsh on 2024-01-29 21:00_

---

_Comment by @charliermarsh on 2024-01-29 21:05_

https://github.com/tokio-rs/tracing/issues/2702

---

_Comment by @zanieb on 2024-07-01 21:47_

Still blocked.

See:

- https://github.com/tokio-rs/tracing/issues/2923
- https://github.com/tokio-rs/tracing/pull/2945
- https://github.com/hawkw/matchers/pull/5

---

_Comment by @j178 on 2025-08-30 00:45_

Should be fixed by https://github.com/astral-sh/uv/pull/15584

---

_Comment by @charliermarsh on 2025-08-30 01:08_

Let's go!

---

_Closed by @charliermarsh on 2025-08-30 01:08_

---
