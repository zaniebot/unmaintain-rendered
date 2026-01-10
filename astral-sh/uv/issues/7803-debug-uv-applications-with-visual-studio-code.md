---
number: 7803
title: Debug uv applications with Visual Studio Code
type: issue
state: closed
author: AndreuCodina
labels:
  - question
assignees: []
created_at: 2024-09-30T05:30:00Z
updated_at: 2025-12-11T14:40:41Z
url: https://github.com/astral-sh/uv/issues/7803
synced_at: 2026-01-10T01:24:19Z
---

# Debug uv applications with Visual Studio Code

---

_Issue opened by @AndreuCodina on 2024-09-30 05:30_

You can use Python or a Python tool (such as `.env/bin/fastapi`) to debug an application in Visual Studio Code. You just have to add a `.vscode/launch.json` file with the configuration. For example, you set a breakpoint and run a FastAPI application with `.venv/bin/fastapi dev src/main.py`.

I've prepared a demo: https://github.com/AndreuCodina/api-to-debug

The issue is this is not possible with `uv`. Therefore, if you update the configuration to:

```json
{
    "version": "0.2.0",
    "configurations": [
      {
        "name": "api",
        "type": "debugpy",
        "request": "launch",
        "program": "uv",
        "args": [
          "run",
          "fastapi",
          "dev",
          "src/main.py"
        ],
        "console": "integratedTerminal",
        "justMyCode": false,
        "preLaunchTask": "uv-sync"
      }
    ]
  }
```

You'll see the next error: `[Errno 2] No such file ot directory: '/Users/me/api-to-debug/uv'`

This issue is also related to https://github.com/astral-sh/uv/issues/5903 because when `uv` has a task runner, it should provide debug capabilities. I've done tests with `poe`, but the breakpoints were ignored. Maybe the tool `fastapi` (@tiangolo) is doing some special to provide debug capabilities.

**uv:** 0.4.17
**OS:** MacOS 15.0

---

_Comment by @zanieb on 2024-09-30 14:01_

Sounds like you need to provide the full path to `uv`? It looks like it's searching for it in your project (and for FastAPI, you show a relative path within the project)

---

_Label `question` added by @zanieb on 2024-09-30 14:01_

---

_Comment by @AndreuCodina on 2024-09-30 15:31_

> Sounds like you need to provide the full path to `uv`? It looks like it's searching for it in your project (and for FastAPI, you show a relative path within the project)

When you do that, you get the error `source code string cannot contain null bytes`. In addition, each OS has its own path (`/opt/homebrew/bin/uv` on MacOS if you installed it using `brew`, `/usr/local/bin/uv` on Linux, etc.).

---

_Comment by @zanieb on 2024-09-30 15:45_

Have you tried creating a symlink to the `uv` binary in your project?

I'm not quite sure what you expect us to change in uv here — this seems like a VS Code usage question

---

_Comment by @AndreuCodina on 2024-09-30 17:23_

> Have you tried creating a symlink to the `uv` binary in your project?
> 
> I'm not quite sure what you expect us to change in uv here — this seems like a VS Code usage question

Well, I think it's about how uv is integrated in all IDEs and it's very important: you need to debug code and tests with your IDE. I don't know what's the solution, but for example, the tool https://fastapi.tiangolo.com/fastapi-cli/ provides debug integration.

The symlink doesn't work:

```bash
$ ln -s /usr/local/bin/uv .venv/bin/uv

$ .venv/bin/uv --help
...
```

Error in VS Code: `source code string cannot contain null bytes`.

---

_Comment by @PhilHem on 2024-10-04 08:07_

VSCode can only run python modules with the module option, as far as I've seen. One workaround for now that I use is to just run `uv add uv`. Then it works. It would be nicer though to not have uv itself in the dependencies.

EDIT: Ok this is not entirely true. It seems to be able to run the script from the launch file, but at least in my case, debugpy does not seem to properly attach to the script.

---

_Comment by @zanieb on 2024-10-21 21:59_

(Not sure this is actionable for us)

---

_Closed by @zanieb on 2024-10-21 21:59_

---

_Referenced in [astral-sh/uv#8558](../../astral-sh/uv/issues/8558.md) on 2024-10-25 13:21_

---

_Comment by @markwitt1 on 2024-11-29 13:50_

Any update on this?

---

_Comment by @michelkok on 2024-11-29 16:06_

I stumbled upon this issue as well. My issue was resolved using [this](https://stackoverflow.com/a/66971688/18276367) SO answer. This is how you can use uv with launch.json.

---

_Comment by @YanSte on 2024-12-22 17:16_

I'm posting this here for those who want to use the FastAPI launcher.

Note: Using uv didn't work, but that give the same results as using the FastAPI module.

```json
{
  "version": "0.0.1",
  "configurations": [
    {
      "name": "API:DEV",
      "type": "debugpy",
      "request": "launch",
      "module": "fastapi",
      "cwd": "${workspaceFolder}/api/api/", # Edit path where main.py
      "args": [
        "dev" # Dev mode
      ],
      "justMyCode": false,
      "env": {
        "ENVIRONMENT": "dev"
      }
    },    
    {
      "name": "API:PROD",
      "type": "debugpy",
      "request": "launch",
      "module": "fastapi",
      "cwd": "${workspaceFolder}/api/api/",  # Edit path where main.py
      "args": [
        "run" # Prod mode
      ],
      "justMyCode": false,
      "env": {
        "ENVIRONMENT": "prod"
      }
    }, 
  ]
}

```

---

_Comment by @xavriley on 2025-01-20 14:19_

For those using a venv created with `uv venv`:

1) Open the Command Palette (shift + apple + p)
2) select "Python: Select Interpreter"
3) select the python version at `.venv/bin/python`

This works with the VSCode debugger

---

_Comment by @kelvincesar on 2025-03-05 22:47_

I could debug using this python env extension: https://marketplace.visualstudio.com/items?itemName=teticio.python-envy

---

_Comment by @brant-qai on 2025-04-03 18:26_

FYI for me, setting the Interpreter to the bin/python in the virtualenv didn't work, but selecting the .venv folder itself did - vscode seemed to understand that meant "use this venv" and all was well from there.

---

_Comment by @tremu on 2025-07-08 19:01_

> FYI for me, setting the Interpreter to the bin/python in the virtualenv didn't work, but selecting the .venv folder itself did - vscode seemed to understand that meant "use this venv" and all was well from there.

you just saved my bacon tysm

---

_Comment by @lambdakris on 2025-08-26 17:34_

I was able to debug a Streamlit app with an environment managed by UV using the following VS Code `launch.json`.

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Streamlit Debugger",
            "type": "debugpy",
            "request": "launch",
            "python": ".venv/bin/python",
            "module": "streamlit",
            "args": ["run", "app/frontend/main.py", "--server.address", "localhost", "--server.port", "8501"]
        }
    ]
}
```

The "secret" is the setting `"python": ".venv/bin/python"` which points the debugger to use the python for the UV managed venv (UV by default creates a venv in `{projectDir}/.venv`) and which picks up any packages etc available in the venv. As far as I can tell, the path `.venv/bin/python` should be constant across os/platforms when using UV, and you can always [customize the venv path for UV](https://docs.astral.sh/uv/pip/environments/#using-arbitrary-python-environments) if necessary.

---

_Referenced in [microsoft/vscode#270323](../../microsoft/vscode/issues/270323.md) on 2025-11-20 08:43_

---

_Comment by @yslbit on 2025-11-26 03:32_

> For those using a venv created with `uv venv`:
> 
> 1. Open the Command Palette (shift + apple + p)
> 2. select "Python: Select Interpreter"
> 3. select the python version at `.venv/bin/python`
> 
> This works with the VSCode debugger

Thanks, for the third step, i succeeded when i selected  ".venv/bin/python3" instead of ".venv/bin/python".

---

_Comment by @jamiekt on 2025-12-11 14:40_

Hi folks,
The solutions here work when the virtualenv created by uv is in a known location. However, when I use `uv run script.py` uv will create an ephemeral environment. This is useful because I'm declaring dependencies in my script as described at https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies

I'm keen to find a way that I can run my script using `uv run` while still using the Code/Cursor debugger.

---
