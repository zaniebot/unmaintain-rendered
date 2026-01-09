---
number: 16718
title: uv run with --project does not run command in project directory as help text says
type: issue
state: closed
author: stewartadam
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2025-11-13T06:50:58Z
updated_at: 2025-12-03T18:15:37Z
url: https://github.com/astral-sh/uv/issues/16718
synced_at: 2026-01-07T13:12:19-06:00
---

# uv run with --project does not run command in project directory as help text says

---

_Issue opened by @stewartadam on 2025-11-13 06:50_

### Summary

```sh
$ uv init foo
Initialized project `foo` at `/Users/stewartadam/foo`
$ cd foo

$ uv init bar
Adding `bar` as member of workspace `/Users/stewartadam/foo`
Initialized project `bar` at `/Users/stewartadam/foo/bar`

$ cd bar
$ uv run --project bar main.py
Using CPython 3.12.12
Creating virtual environment at: .venv
Hello from foo!

$ uv run --help | grep -e --project
      --project <PROJECT>                          Run the command within the given project directory [env: UV_PROJECT=]
```

Given the help text, I would have expected this to return "hello from bar". I'm guessing the current behavior is correct, so we may want to adjust the help text to make clear it's not actually running it within the given project directory, but rather just using the project context.



### Platform

macOS 14

### Version

uv 0.9.9 (Homebrew 2025-11-12)

### Python version

Python 3.12.12

---

_Label `bug` added by @stewartadam on 2025-11-13 06:51_

---

_Comment by @nooscraft on 2025-11-13 08:21_

@zanieb Are we going ahead with the help text adjustments that @stewartadam suggested? If so, I can take a look at it.

---

_Comment by @charliermarsh on 2025-11-28 14:34_

I think we can tweak the text a bit. The flag you're looking for (that essentially cd's to the directory) is `--directory`. `--project` just makes it such that we perform file discovery from the given directory, i.e., identify the current project based on that path.

---

_Label `bug` removed by @charliermarsh on 2025-11-28 14:34_

---

_Label `documentation` added by @charliermarsh on 2025-11-28 14:34_

---

_Comment by @nooscraft on 2025-12-03 13:32_

@charliermarsh, both are feasible. We could update the help text to clarify that `--project` affects file discovery vs `--directory` changing the working dir. 

For the behavior change, we can set the working directory before spawning the command. Updating the help text would be safer and fixes the confusion.

---

_Comment by @zanieb on 2025-12-03 13:36_

Yeah I think

> Discover a project in the given directory

is reasonable

---

_Label `good first issue` added by @zanieb on 2025-12-03 13:36_

---

_Referenced in [astral-sh/uv#16965](../../astral-sh/uv/pulls/16965.md) on 2025-12-03 13:54_

---

_Closed by @zanieb on 2025-12-03 18:15_

---
