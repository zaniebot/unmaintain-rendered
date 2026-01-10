---
number: 11798
title: "uv tool run can't find installed tool"
type: issue
state: closed
author: superlopuh
labels:
  - question
assignees: []
created_at: 2025-02-26T13:41:26Z
updated_at: 2025-02-26T15:43:17Z
url: https://github.com/astral-sh/uv/issues/11798
synced_at: 2026-01-10T01:25:10Z
---

# uv tool run can't find installed tool

---

_Issue opened by @superlopuh on 2025-02-26 13:41_

### Summary

Hello, I might be misunderstanding how `uv tool run` should work, but I'm confused by this message:

```
❯ uv tool list
codecov-cli v10.1.1
- codecov
- codecovcli
❯ uv tool run codecovcli
  × No solution found when resolving tool dependencies:
  ╰─▶ Because codecovcli was not found in the package registry and you require codecovcli, we can conclude that your requirements are unsatisfiable.
```

I just ran `uv tool install codecov-cli`, which succeeded, but actually running `codecovcli` does not.

### Platform

macOS

### Version

uv 0.6.2 (Homebrew 2025-02-19)

### Python version

Python 3.13.1

---

_Label `bug` added by @superlopuh on 2025-02-26 13:41_

---

_Comment by @zanieb on 2025-02-26 15:08_

After installation, the tool is on your `PATH` and you can just use `codecovcli` directly.

If you want to use `uv tool run`, you need to provide the package name since it differs from the command name, e.g., `uv tool run --from codecov-cli codecovcli`

---

_Label `bug` removed by @zanieb on 2025-02-26 15:08_

---

_Label `question` added by @zanieb on 2025-02-26 15:08_

---

_Comment by @superlopuh on 2025-02-26 15:43_

Thank you!

---

_Closed by @superlopuh on 2025-02-26 15:43_

---
