---
number: 12710
title: docker build cache mount example gives permissions error
type: issue
state: closed
author: marengaz
labels:
  - question
assignees: []
created_at: 2025-04-07T09:08:14Z
updated_at: 2025-04-07T10:00:31Z
url: https://github.com/astral-sh/uv/issues/12710
synced_at: 2026-01-07T13:12:18-06:00
---

# docker build cache mount example gives permissions error

---

_Issue opened by @marengaz on 2025-04-07 09:08_

### Question

not necessarily a `uv` problem, but potentially some docs that could be improved...

when i docker build, this line fails
https://github.com/astral-sh/uv/blob/main/docs/guides/integration/docker.md?plain=1#L419-L422

with the error
```
0.455 error: failed to create directory `/root/.cache/uv`: Permission denied (os error 13)
```

im not sure how to solve it. perhaps you could advise and add a note in the docs?

### Platform

macos 15.3.2 m3 pro. also tried on google cloud build managed runners

### Version

uv 0.6.12 (`latest` in the dockerfile)

---

_Label `question` added by @marengaz on 2025-04-07 09:08_

---

_Comment by @marengaz on 2025-04-07 10:00_

my bad. i forgot i changed the first `FROM <image>` . that image had specified a non-root user.

---

_Closed by @marengaz on 2025-04-07 10:00_

---
