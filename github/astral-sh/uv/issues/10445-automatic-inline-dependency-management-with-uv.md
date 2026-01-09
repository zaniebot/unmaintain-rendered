---
number: 10445
title: Automatic Inline Dependency Management with uv add and uv sync
type: issue
state: closed
author: hmrc87
labels: []
assignees: []
created_at: 2025-01-09T21:03:08Z
updated_at: 2025-01-11T13:55:45Z
url: https://github.com/astral-sh/uv/issues/10445
synced_at: 2026-01-07T13:12:18-06:00
---

# Automatic Inline Dependency Management with uv add and uv sync

---

_Issue opened by @hmrc87 on 2025-01-09 21:03_

I want to suggest a feature to allow UV to automatically manage packages as inline dependencies in a designated file (e.g., main.py).


1. When a package is added with _uv add_, it will also be appended to the dependencies list in the designated file (if UV_AUTO_ADD_INLINE_DEPS is set).
2. Integration with _uv sync_: A similar mechanism will apply to uv sync when a corresponding flag or environment variable is set. 

The feature can be enabled via a CLI-Flag  or as an environment variable UV_AUTO_MANAGE_INLINE_DEPS

**Example**
```
# uv
uv add  requests --auto-add-inline-deps 

# adds the following block to the py file that was added  with "uv init"
 

/// script  
 dependencies = [  
   "requests",  
 ]  
 ///  
```

This can be useful in scenarios where a developer wants to share the python file without providing the _pyproject.toml_. 

Probably not the most common usecase but there might be additional benefits, can you think of one?

**Open Questions**
- Should a specific file (e.g., main.py) always be targeted, or should this be configurable?
- How should duplicate dependencies or conflicts be handled if they already exist in the inline list?


---

_Comment by @zanieb on 2025-01-09 21:08_

There's `uv add --script <path>`

I'm not sure we'll provide a way to automatically sync all the project dependencies to a script on every invocation.

---

_Comment by @zanieb on 2025-01-09 21:42_

Following #10447 the following will be allowed

```
uv export | uv add -r - --script main.py
```

---

_Assigned to @zanieb by @zanieb on 2025-01-09 21:50_

---

_Closed by @zanieb on 2025-01-09 22:57_

---

_Comment by @hmrc87 on 2025-01-11 13:55_

That's a great idea and already merged? You rock @zanieb!

---
