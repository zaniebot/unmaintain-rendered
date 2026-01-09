---
number: 13875
title: Why are so many regular logs using printer.stderr?
type: issue
state: closed
author: NonsenseSynapse
labels:
  - question
assignees: []
created_at: 2025-06-05T23:47:11Z
updated_at: 2025-06-06T17:36:46Z
url: https://github.com/astral-sh/uv/issues/13875
synced_at: 2026-01-07T13:12:18-06:00
---

# Why are so many regular logs using printer.stderr?

---

_Issue opened by @NonsenseSynapse on 2025-06-05 23:47_

### Question

I have a Python application that runs uv syncs downstream and writes all of the output to a log file. I noticed recently that a number of seemingly innocuous log messages were being flagged as an error log level. For example:
```
2025-06-05 16:11:51,605 com.my.app.name ERROR Resolved 23 packages in 1.74s
2025-06-05 16:11:51,681 com.my.app.name ERROR Installed 22 packages in 42ms
```

Looking into the codebase, I found the above messages and they are, in fact, [being printed with printer.stderr](https://github.com/astral-sh/uv/blob/7dd564d3d09668488c179b02bfe446754e58a5ea/crates/uv/src/commands/pip/loggers.rs#L103)

Based on the source code, it seems like this is intentional, but as a developer this feels very unintuitive. Conventionally, I have viewed ERROR-level logs as an indication that something has failed to a degree that the expected behavior of your program is impaired. But in this case, they are regular messages that are expected and show up with every `uv sync`.

I was wondering if any maintainers could provide insight into why this decision was made?

### Platform

macOS 14.4.1

### Version

uv 0.6.13 (a0f5c7250 2025-04-07)

---

_Label `question` added by @NonsenseSynapse on 2025-06-05 23:47_

---

_Comment by @zanieb on 2025-06-06 03:39_

stderr is the standard stream for logs, stdout is used for piping data to other programs.

---

_Comment by @zanieb on 2025-06-06 03:40_

We've talked about this briefly before, e.g., in https://github.com/astral-sh/uv/issues/2656

This isn't a uv-thing, this is typical behavior for command-line tooling.

---

_Comment by @NonsenseSynapse on 2025-06-06 17:09_

Ahh interesting. Thanks for the quick response!

---

_Closed by @zanieb on 2025-06-06 17:36_

---
