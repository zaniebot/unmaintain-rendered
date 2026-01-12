```yaml
number: 3214
title: Avoid Removing Quotes From Requirement Markers
type: pull_request
state: merged
author: ibraheemdev
labels:
  - bug
assignees: []
merged: true
base: main
head: stray-quotes-fix
created_at: 2024-04-23T15:26:59Z
updated_at: 2024-04-23T18:08:07Z
url: https://github.com/astral-sh/uv/pull/3214
synced_at: 2026-01-12T16:05:30Z
```

# Avoid Removing Quotes From Requirement Markers

---

_@ibraheemdev_

## Summary

Avoid removing quotes from markers, e.g. `numpy (>=1.19) ; python_version >= "3.7"` should not be rewritten. Fixes https://github.com/astral-sh/uv/issues/2551.

This PR also makes fixups a bit more flexible internally for fixes that aren't simple to implement with a pure regex replacement, like this one. https://github.com/astral-sh/uv/pull/1529 fixed a similar problem but the current regex is still not smart enough to avoid all markers completely (like `python_version`).

## Test Plan

Added a few unit tests.

---

_Label `bug` added by @konstin on 2024-04-23 15:57_

---

_@BurntSushi approved on 2024-04-23 16:44_

Nice! This is much simpler.

---

_Merged by @charliermarsh on 2024-04-23 18:07_

---

_Closed by @charliermarsh on 2024-04-23 18:07_

---

_Comment by @charliermarsh on 2024-04-23 18:08_

(Gonna go out-of-turn and merge this here since it also includes fixes to get `main` passing.)

---
