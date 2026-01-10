---
number: 10854
title: netrc parsing errors are silently swallowed
type: issue
state: open
author: reuben
labels:
  - question
assignees: []
created_at: 2025-01-22T12:43:38Z
updated_at: 2025-01-22T15:24:45Z
url: https://github.com/astral-sh/uv/issues/10854
synced_at: 2026-01-10T01:24:58Z
---

# netrc parsing errors are silently swallowed

---

_Issue opened by @reuben on 2025-01-22 12:43_

### Summary

When the .netrc file contains a syntax error uv will simply say that the index couldn't be queried due to an authentication failure, you have to look into the `RUST_LOG=debug` output to figure out that .netrc parsing failed and that's why the credentials weren't picked up.

### Platform

Ubuntu 22.04 amd64

### Version

uv 0.5.21

### Python version

Python 3.10.15

---

_Label `bug` added by @reuben on 2025-01-22 12:43_

---

_Comment by @zanieb on 2025-01-22 15:24_

I think it's expected that `-v` is required to see netrc parsing errors, since it's not opt-in.

---

_Label `bug` removed by @zanieb on 2025-01-22 15:24_

---

_Label `question` added by @zanieb on 2025-01-22 15:24_

---
