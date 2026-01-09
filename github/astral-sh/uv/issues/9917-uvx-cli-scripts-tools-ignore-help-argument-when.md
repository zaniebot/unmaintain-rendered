---
number: 9917
title: "uvx CLI scripts (\"tools\" ?) ignore `--help` argument when using `Fire` or `Typer`"
type: issue
state: closed
author: pchalasani
labels:
  - question
assignees: []
created_at: 2024-12-15T16:36:54Z
updated_at: 2024-12-15T17:24:53Z
url: https://github.com/astral-sh/uv/issues/9917
synced_at: 2026-01-07T13:12:18-06:00
---

# uvx CLI scripts ("tools" ?) ignore `--help` argument when using `Fire` or `Typer`

---

_Issue opened by @pchalasani on 2024-12-15 16:36_

E.g. in my `langroid-examples` repo, I've enabled some scripts to be runnable as follows:

```bash
uvx --from langroid-examples chat
```

I've set up a `[project.scripts]` table with these entries:
```toml
[project.scripts]
chat = "examples.basic.chat:app"
completion = "examples.basic.completion:app"
chatdoc = "examples.docqa.chat:app"
chatsearch = "examples.docqa.chat_search:main"
```

See [here](https://github.com/langroid/langroid-examples?tab=readme-ov-file#run-scripts-with-no-repo-cloning-no-venv-setup) and the actual `chat.py` script [here](https://github.com/langroid/langroid-examples/blob/main/examples/basic/chat.py)

Note that I use `typer.Typer.app()` for running this app. 
Running `uvx --from langroid-examples chat` works fine, but when I run
```bash
uvx --from langroid-examples chat --help
```
The `--help` is ignored. 
Similarly, the `chat_search.py` uses `fire.Fire` to create the cli, and there as well the `--help` is ignored.

I don't know if this problem is specific to `uv`, or in general `--help` is ignored when CLI scripts are defined using `Fire` or `Typer`

---

_Comment by @zanieb on 2024-12-15 16:57_

I don't see how this could be caused by uv. I'd recommend trying to reproduce with pip, e.g.,

```
uv venv --seed
source .venv/bin/activate
pip install langroid-examples
chat --help
```

---

_Label `question` added by @zanieb on 2024-12-15 16:57_

---

_Comment by @pchalasani on 2024-12-15 17:11_

You're right, `--help` is ignored, so it must be an issue with Fire/Typer

---

_Closed by @pchalasani on 2024-12-15 17:24_

---
