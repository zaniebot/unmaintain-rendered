---
number: 11022
title: Adding root directory to sys.path
type: issue
state: closed
author: haikesan
labels:
  - enhancement
assignees: []
created_at: 2025-01-28T16:00:31Z
updated_at: 2025-02-02T18:25:22Z
url: https://github.com/astral-sh/uv/issues/11022
synced_at: 2026-01-07T13:12:18-06:00
---

# Adding root directory to sys.path

---

_Issue opened by @haikesan on 2025-01-28 16:00_

### Summary

When running script from uv environment, it doesn't add project root into sys.path, as does poetry, for example.
Having such structure
app
  ...
tests
  script.py

And in this script having something like 

```py
from app.module import something

...
```

Breaks, because in sys.path only "tests" folder is added, not really ".", while it is really handy. Maybe any workaround for that?

I know about sys.path.append("."), or running python -m tests.script, but it's not solution

### Example

_No response_

---

_Label `enhancement` added by @haikesan on 2025-01-28 16:00_

---

_Comment by @haikesan on 2025-01-28 16:23_

Also, adding current project to dependencies doesn't help, and such deps manipulation is NOT a preferred solution

---

_Comment by @zanieb on 2025-01-28 16:24_

Can you share a complete reproducible example? e.g.,

```
uv init example
cd example
mkdir -p app/module
touch app/module/__init__.py
echo "import app.module" > script.py
uv run script.py
```

---

_Comment by @haikesan on 2025-01-28 16:35_

Here are two examples - with uv and poetry. Poetry's sys.path is automatically extended with project root, and with uv this doesn't happen (you can see it via my example)
[reproduce_11022.zip](https://github.com/user-attachments/files/18576515/reproduce_11022.zip)

---

_Comment by @haikesan on 2025-01-28 16:44_

Also, I am running them in this way:
for uv
```
cd uv_example
uv sync
. .venv/bin/activate
python tests/script.py
```
Getting error here, then
```
deactivate 
cd ..
```

for poetry
```
cd poetry_example
poetry install
poetry shell
python tests/script.py
```
And everything works as it should, while also having project root in sys.path

```
deactivate 
cd ..
```

Running via uv run also doesn't solve problem. It's something with pythonpath or activation process, I guess

---

_Comment by @uwu-420 on 2025-01-29 15:44_

Hi @hRtWzFe I would recommend not providing your examples as .zip file but as a a public repository or if it's small enough just as code blocks in the comments with the full file path per file. I'm afraid, but opening a .zip file from a stranger on the internet is kinda scary 😅 

---

_Comment by @haikesan on 2025-01-29 18:00_

Here is repo to reproduce:
https://github.com/hRtWzFe/uv-issue-11022

---

_Comment by @haikesan on 2025-02-02 18:25_

I think I found the solution. 

Initializing project as package with `uv init --package`

Or adding

```
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build.targets.wheel]
packages = ["."]
```

if you already have your codebase (without src) solves this problem!

---

_Closed by @haikesan on 2025-02-02 18:25_

---
