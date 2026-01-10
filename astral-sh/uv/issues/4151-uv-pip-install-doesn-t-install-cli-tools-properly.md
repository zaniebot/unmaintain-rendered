---
number: 4151
title: "`uv pip install` doesn't install CLI tools properly"
type: issue
state: closed
author: Malix-Labs
labels:
  - question
  - needs-mre
assignees: []
created_at: 2024-06-07T22:55:34Z
updated_at: 2025-05-27T10:10:49Z
url: https://github.com/astral-sh/uv/issues/4151
synced_at: 2026-01-10T01:23:35Z
---

# `uv pip install` doesn't install CLI tools properly

---

_Issue opened by @Malix-Labs on 2024-06-07 22:55_

`uv pip install` doesn't install CLI tools properly

I.e.
`uv pip install fastpi` doesn't install and setup `fastapi-cli` properly

---

_Comment by @zanieb on 2024-06-07 23:18_

Sorry, but we need way more details here for this issue to be actionable. Can you please describe how it's not setup properly, compare to pip, etc.? 

---

_Label `needs-mre` added by @zanieb on 2024-06-07 23:18_

---

_Label `question` added by @zanieb on 2024-06-07 23:18_

---

_Comment by @Malix-Labs on 2024-06-07 23:45_

For example, `pip install fastapi` also installs `fastapi-cli` and adds it to the shell automatically (useful for `fastapi dev` for example);
However, `uv pip install fastapi` doesn't do that

---

_Comment by @zanieb on 2024-06-07 23:49_

We don't add commands to the shell automatically because they're installed into a virtual environment. Activate the environment and it'll be on your shell:

```
❯ uv venv
Using Python 3.12.3 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
❯ uv pip install fastapi
...
❯ source .venv/bin/activate
❯ which fastapi
/Users/zb/workspace/uv/.venv/bin/fastapi
❯ fastapi --help
                                                                                                                             
 Usage: fastapi [OPTIONS] COMMAND [ARGS]...                                                                                  
                                                                                                                             
 FastAPI CLI - The fastapi command line app. 😎     
```

---

_Comment by @charliermarsh on 2024-06-07 23:49_

👍 Working as expected for me.

---

_Comment by @zanieb on 2024-06-07 23:52_

Perhaps with pip you're experiencing system or user level installation, which we do not do by default. See our [pip compatibility section on this](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#virtual-environments-by-default) for more details.

---

_Comment by @Malix-Labs on 2024-06-08 18:22_

Indeed, it was a problem from by bash script executing the venv activation script in a sub-process and terminating it instead of doing it in the top script, I apologize for that

---

_Closed by @Malix-Labs on 2024-06-08 18:22_

---

_Referenced in [skypilot-org/skypilot#5181](../../skypilot-org/skypilot/issues/5181.md) on 2025-04-10 21:17_

---

_Referenced in [skypilot-org/skypilot#4952](../../skypilot-org/skypilot/issues/4952.md) on 2025-04-15 21:03_

---

_Comment by @PantiL on 2025-05-27 10:07_

Hello.
I encountered the same problem. I repeated the installation commands exactly as indicated here.
```
$ uv init my_project
Initialized project 'my-project' at '/home/pan/projects/fastapi/my_project'
$ cd my_project/
$ uv venv
Using CPython 3.12.3 interpreter at: /usr/bin/python3.12
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
$ uv pip install fastapi
Resolved 10 packages in 1.50s
Installed 10 packages in 11ms
 + annotated-types==0.7.0
 + anyio==4.9.0
 + fastapi==0.115.12
 + idna==3.10
 + pydantic==2.11.5
 + pydantic-core==2.33.2
 + sniffio==1.3.1
 + starlette==0.46.2
 + typing-extensions==4.13.2
 + typing-inspection==0.4.1
$ source .venv/bin/activate
$ which fastapi
/home/pan/projects/fastapi/my_project/.venv/bin/fastapi
$ fastapi --help
To use the fastapi command, please install "fastapi[standard]":

	pip install "fastapi[standard]"

Traceback (most recent call last):
  File "/home/pan/projects/fastapi/my_project/.venv/bin/fastapi", line 10, in <module>
    sys.exit(main())
             ^^^^^^
  File "/home/pan/projects/fastapi/my_project/.venv/lib/python3.12/site-packages/fastapi/cli.py", line 12, in main
    raise RuntimeError(message)  # noqa: B904
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^
RuntimeError: To use the fastapi command, please install "fastapi[standard]":

	pip install "fastapi[standard]"
```
But the problem remains.
The problem was solved only by installing fastapi-cli manually
```
$ uv pip install fastapi-cli
Resolved 21 packages in 7.32s
Installed 17 packages in 16ms
 + click==8.2.1
 + fastapi-cli==0.0.7
 + h11==0.16.0
 + httptools==0.6.4
 + markdown-it-py==3.0.0
 + mdurl==0.1.2
 + pygments==2.19.1
 + python-dotenv==1.1.0
 + pyyaml==6.0.2
 + rich==14.0.0
 + rich-toolkit==0.14.6
 + shellingham==1.5.4
 + typer==0.16.0
 + uvicorn==0.34.2
 + uvloop==0.21.0
 + watchfiles==1.0.5
 + websockets==15.0.1
```


---

_Referenced in [astral-sh/uv#13674](../../astral-sh/uv/issues/13674.md) on 2025-05-27 10:18_

---
