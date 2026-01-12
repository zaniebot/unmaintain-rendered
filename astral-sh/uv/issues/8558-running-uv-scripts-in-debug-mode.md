```yaml
number: 8558
title: Running uv scripts in debug mode
type: issue
state: open
author: RubenVanEldik
labels:
  - question
assignees: []
created_at: 2024-10-25T09:04:48Z
updated_at: 2025-12-15T21:19:11Z
url: https://github.com/astral-sh/uv/issues/8558
synced_at: 2026-01-12T15:59:29Z
```

# Running uv scripts in debug mode

---

_@RubenVanEldik_

Hi all!

I am trying to see if we can start using uv to manage our codebases. Everything seems to work great, and managing dependencies is a lot easier this way! However, I can't figure out how to run a program through uv in debug mode in either VS Code or PyCharm. (e.g., `uv run src/main.py`) Does anyone have experience with running uv scripts in debug mode and can you point me in the right direction? :)

Thank you!

---

_Comment by @zanieb on 2024-10-25 13:21_

Related to https://github.com/astral-sh/uv/issues/7803

We don't have a great recommendation here yet.

---

_Label `question` added by @zanieb on 2024-10-25 13:21_

---

_Comment by @emilyemorehouse on 2024-10-25 19:42_

I configured an alias (`alias python="uv run python"`) and am able to debug apps/scripts without any modification to my VS Code config (and without forcing other devs to use `uv` if they aren't ready to migrate yet). I do feel like this is more of a workaround and has its pros/cons, but I'm happy with using this as a working solution to remove the barrier to utilizing `uv`.

---

_Comment by @RubenVanEldik on 2024-10-29 10:55_

That's a great workaround @emilyemorehouse! I will definitely be using this myself as well. However, before I can convince the rest of my team that we should start using uv everywhere, it is important to have a less 'hacky' way to debug uv programs.

@zanieb, I fully understand this is not at the top of the priority list of uv right now, but I do think it is important to have some sort of solution for this when uv goes more mainstream :)

---

_Comment by @franperezlopez on 2024-11-01 14:20_

I agree with @RubenVanEldik : if astral doesn't support common development scenarios (such as debug using vscode), all the other efforts will be meaningless. 
![image](https://github.com/user-attachments/assets/fd35d662-dcbc-444f-9624-034924697024)
another way to see it: now vscode offers support for pip (pipx, ...) and conda (micromamba, ...) environments, so the day I see UV in that list, then I will talk about UV to other developers. I hope that day comes soon

---

_Comment by @zanieb on 2024-11-01 14:58_

I understand the desire, but it seems like this is sort of something for the VSCode team to tackle. I don't understand the nuances of the debugger setup and if `alias python="uv run python"` works then it seems like there's not really a technical limitation just a weird constraint on how the Python interpreter is invoked?

---

_Comment by @gresavage on 2024-11-01 17:56_

> I understand the desire, but it seems like this is sort of something for the VSCode team to tackle. I don't understand the nuances of the debugger setup and if `alias python="uv run python"` works then it seems like there's not really a technical limitation just a weird constraint on how the Python interpreter is invoked?

I second this!

From what I can see, UV obeys the *ancient* [PEP0405](https://peps.python.org/pep-0405/) so any issues with VSCode or PyCharm selecting the correct interpreter stem from the IDE, not UV.

FWIW I also had this issue initially and the `alias python="uv run python"` hack worked, but was actually no longer needed after restarting VSCode... Perhaps even just restarting the extension host would suffice.

---

_Comment by @gresavage on 2024-11-01 18:07_

@franperezlopez UV abides by [PEP0405](https://peps.python.org/pep-0405/). So the UV virtual environment is literally the same as choosing the `Venv` from the dropdown in the image you shared... it's nothing special and you should just be able to select the correct interpreter for the project as I have done (below).

UV will create a new virtual environment on `uv sync` or you can create one manually with `uv venv`. The resulting venv should appear as an option when selecting an interpreter for the project. I think you could even just choose the `Venv` option you showed, and UV will automatically sync packages to that venv.

I did have issues initially with VS Codes debugger using the right interpreter - but restarting VS Code fixed this... perhaps even just restarting the extension host would work. This strongly plus UVs PEP compliance indicates to me the issue lies with VS Code and not UV.

![image](https://github.com/user-attachments/assets/d32f8021-615b-45c0-9858-c90264c58e1e)


---

_Comment by @AlexeyPetrochenko on 2024-11-05 15:49_


You can run your FastAPI application using the following configuration in launch.json:

```json
{
    "name": "api",
    "type": "debugpy",
    "request": "launch",
    "module": "app",
    "console": "internalConsole",
    "justMyCode": true
}
```

In this configuration, "app" refers to the Python module that contains the file __main__.py.
__main__.py
Create a file named __main__.py with the following content:

```python
import uvicorn

if __name__ == '__main__':
    uvicorn.run("app.server:create_app", host="127.0.0.1", port=8000, reload=True, factory=True)
```

Here, server.py is the entry point of the application, and create_app is the application factory function. Using a factory pattern is recommended for better flexibility.
server.py
In your server.py, define the application factory as follows:

```python
from fastapi import FastAPI

def create_app() -> FastAPI:
    app = FastAPI()
    return app
```

Running the Application
With this setup, you can run your FastAPI application in debug mode directly from Visual Studio Code. Simply select the configuration named "api" from the debug panel and start debugging.
This configuration allows you to take advantage of debugging features such as breakpoints and variable inspection while developing your FastAPI application.
Final Notes
Ensure Dependencies: Make sure you have all necessary dependencies installed, including FastAPI and Uvicorn.
Directory Structure: Ensure that your directory structure supports this setup, with __main__.py located in the correct module path.
```
├── app/                     
│   ├── __init__.py          
│   ├── __main__.py       
│   └── server.py 
```

---

_Comment by @adithyakirank on 2024-11-08 15:12_

I think the more vs-code way of doing this until support for uv is added is:

1. Select the interpreter (`uv run which python` if you are not sure) with the command palette like [here](https://code.visualstudio.com/docs/python/environments#_select-and-activate-an-environment)
 
2. Add the launch config for a script at `${workspaceFolder}/tests/test_script.py` to `launch.json`

```
{
    "name": "my_test_script",
    "request": "launch",
    "cwd": "${workspaceFolder}/tests",
    "type":"debugpy",
    "program": "src/tests/test_script.py",
    "console": "integratedConsole",
    "args": [
        "arg1": "value1"
    ]
},
```

---

_Comment by @emilyemorehouse on 2024-11-08 20:44_

> FWIW I also had this issue initially and the `alias python="uv run python"` hack worked, but was actually no longer needed after restarting VSCode... Perhaps even just restarting the extension host would suffice.

This is exactly what happened to me. I've also ditched the alias and simply selecting the right interpreter in VS Code (from the `venv` that I create with uv and auto-activate with direnv) is working seamlessly.

I also agree that this isn't a uv issue – it's up to each debugger/editor to support uv-specific things that don't fit into this box.

---

_Comment by @lugi0 on 2024-12-10 12:19_

I bumped into this while trying to debug pytest execution within a `uv` environment. If you need to do the same, this launch.json file should work for you:

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "justMyCode": false,
            "name": "uv+pytest",
            "type": "debugpy",
            "request": "launch",
            "program": ".venv/bin/pytest",  #path to your venv's pytest bin
            "python": "${command:python.interpreterPath}",  #assuming the correct interpreter is already selected in VSC
            "console": "integratedTerminal",
            "args": "path/to/test.py"  #path to the test you want to run; can be a list; these are the args passed to pytest
        }
    ]
}
```

---

_Comment by @aplantinga on 2025-01-09 10:13_

For VS code, I added `uv sync` as a preLaunchtask to make sure my environment is up-to-date.

`launch.json`:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python Debugger: Current File",
            "type": "debugpy",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "preLaunchTask": "uv_sync"
        }
    ]
}
```

`tasks.json`:
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "uv_sync",
            "type": "shell",
            "command": "uv sync"
        }
    ]
}
```

---

_Comment by @hoegge on 2025-02-18 07:52_

These workarounds do not work with `uv run script.py` where the dependencies are included using uv's script dependencies, do they?
Where scripting is often where it is useful to try out things and debug. Any suggestions on how to run that from VScode easily in debug mode?

```
 /// script
# dependencies = [
#   "pandasgui",
#   "pandas",
#   "psycopg2",****
```

---

_Comment by @say4n on 2025-07-22 12:44_

@hoegge seems like you can use `uv sync --script <SCRIPT>` at least on `uv 0.7.21 (Homebrew 2025-07-14)`. This command also helpfully prints the directory where the virtual environment lives if you want to manually explore things.

---

_Comment by @zanieb on 2025-07-22 12:48_

You can also get the path with `uv sync --script example.py --dry-run --output-format json | jq -r '.sync.environment.path'`

---

_Comment by @mlbiche on 2025-08-19 12:46_

@hoegge FYI, thanks to @say4n and @zanieb indications, I could run from VSCode Debugger my Python script with `uv` script dependencies by:
- Selecting as interpreter the `python` bin in `bin/python` in the `uv` environment directory printed by the `uv sync --script ...` command.
- Setting the `launch.json` according to @adithyakirank indications to run my Python script. *(Note that it is `"console": "internalConsole"` and not `"console": "integratedConsole"` as stated in its comment)*

Hope it may be useful to anyone around here.

---

_Comment by @adithyakirank on 2025-08-19 12:54_

> [@hoegge](https://github.com/hoegge) FYI, thanks to [@say4n](https://github.com/say4n) and [@zanieb](https://github.com/zanieb) indications, I could run from VSCode Debugger my Python script with `uv` script dependencies by:
> 
> * Selecting as interpreter the `python` bin in `bin/python` in the `uv` environment directory printed by the `uv sync --script ...` command.
> * Setting the `launch.json` according to [@adithyakirank](https://github.com/adithyakirank) indications to run my Python script. _(Note that it is `"console": "internalConsole"` and not `"console": "integratedConsole"` as stated in its comment)_
> 
> Hope it may be useful to anyone around here.

My bad, the console options are indeed one of the following values:

`console - what kind of console to use, for example, internalConsole, integratedTerminal, or externalTerminal`

---

_Comment by @RafaelWO on 2025-09-29 12:00_

> These workarounds do not work with uv run script.py where the dependencies are included using uv's script dependencies, do they?

If you are also fine with debugging with [pdb](https://docs.python.org/3/library/pdb.html), you can simply add a `breakpoint()` statement in your Python script and run `uv sync --script <SCRIPT>` as usual. This will stop the program at the breakpoint and leave you with the interactive pdb session.

---

_Comment by @fionn-r on 2025-10-01 03:34_

Yet another way that has worked well for me.

It uses the `python` and `pythonArgs` values to substitute in `uv run python` instead of the default interpreter so it should mean that it's equivalent to running `uv run python ${file}`

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug with uv",
            "type": "debugpy",
            "request": "launch",
            "python": "uv",
            "pythonArgs": [
                "run",
                "python"
            ],
            "program": "${file}"
        }
    ]
}
```

---

_Comment by @joseribon on 2025-10-03 22:36_

Thanks!!! @emilyemorehouse 🧠

---

_Comment by @juanbretti on 2025-10-06 18:01_

> Yet another way that has worked well for me.
> 
> It uses the `python` and `pythonArgs` values to substitute in `uv run python` instead of the default interpreter so it should mean that it's equivalent to running `uv run python ${file}`
> 
> {
>     "version": "0.2.0",
>     "configurations": [
>         {
>             "name": "Debug with uv",
>             "type": "debugpy",
>             "request": "launch",
>             "python": "uv",
>             "pythonArgs": [
>                 "run",
>                 "python"
>             ],
>             "program": "${file}"
>         }
>     ]
> }

Hello, I get this error message when trying to use your proposed `launch.json`.

<img width="352" height="125" alt="Image" src="https://github.com/user-attachments/assets/091a5b72-8efb-4724-8801-7c67c89c43b9" />

Could you give me advice if I need to change something? Or add something to my environment?
Thanks!

---

_Comment by @fionn-r on 2025-10-07 08:38_

> > Yet another way that has worked well for me.
> > It uses the `python` and `pythonArgs` values to substitute in `uv run python` instead of the default interpreter so it should mean that it's equivalent to running `uv run python ${file}`
> > {
> > "version": "0.2.0",
> > "configurations": [
> > {
> > "name": "Debug with uv",
> > "type": "debugpy",
> > "request": "launch",
> > "python": "uv",
> > "pythonArgs": [
> > "run",
> > "python"
> > ],
> > "program": "${file}"
> > }
> > ]
> > }
> 
> Hello, I get this error message when trying to use your proposed `launch.json`.
> 
> <img alt="Image" width="352" height="125" src="https://private-user-images.githubusercontent.com/10165911/497937891-091a5b72-8efb-4724-8801-7c67c89c43b9.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NTk4MjU4OTQsIm5iZiI6MTc1OTgyNTU5NCwicGF0aCI6Ii8xMDE2NTkxMS80OTc5Mzc4OTEtMDkxYTViNzItOGVmYi00NzI0LTg4MDEtN2M2N2M4OWM0M2I5LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTEwMDclMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUxMDA3VDA4MjYzNFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTA1NzA4OWRlNmVhMDA2NjIwMTI2NTUxNGI2ZjZlYTNmMWE4MGIyOTBjNDg2NzhmZTdkYTdjNzUzZmI4MThkOGUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.2LWe754ykn_8q_el1_t_cjIigD0ZCaKE8LttOfMD3tY">
> Could you give me advice if I need to change something? Or add something to my environment? Thanks!

Are you able to provide the output of the `Python Debug Console` terminal that spawns when starting the debug task? Also can include Output > Python and Output > Python Debugger.


Potentially something to do with things not being found but when I intentionally change any of the values in there to pretend they can't be found, I get a pretty clear error message.


---

_Comment by @juanbretti on 2025-10-07 08:56_

Thank you @fionn-r  for your reply.

Using the `launch.json`:

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug with uv",
            "type": "debugpy",
            "request": "launch",
            "python": "uv",
            "pythonArgs": [
                "run",
                "python"
            ],
            "program": "${file}"
        }
    ]
}
```

The outputs I got when trying to debug are:

Python Debugger

```
2025-10-07 10:47:07.840 [info] Resolving launch configuration with substituted variables
```

Python Locator
```
2025-10-07 10:48:35.022 [info] Resolved Python Environment C:\Users\xyz\.venv\Scripts\python.exe
2025-10-07 10:48:35.113 [error] Failed to execute Python to resolve info "\"c:\\Users\\xyz\\.venv\\Scripts\\python.exe\"": El nombre de archivo, el nombre de directorio o la sintaxis de la etiqueta del volumen no son correctos. (os error 123)
2025-10-07 10:48:35.113 [warning] Unknown Python Env "\"c:\\Users\\xyz\\.venv\\Scripts\\python.exe\""
2025-10-07 10:48:35.113 [error] Failed to resolve env "\"c:\\Users\\xyz\\.venv\\Scripts\\python.exe\""
```

But the `python.exe´ it is at that location. I triple checked.

The error is:

<img width="352" height="125" alt="Image" src="https://github.com/user-attachments/assets/a2f32133-5e2d-43c5-9b7c-7136fd368384" />

And the debug does not start.

I am using uv:
```
uv 0.8.23 (00d3aa378 2025-10-04)
```

When VScode starts, the Python locator find `python.exe` without any error:
```
2025-10-07 10:53:03.475 [info] Resolved Python Environment C:\Users\xyz\.venv\Scripts\python.exe
```

Any ideas what could be wrong?

---

_Comment by @fionn-r on 2025-10-07 10:35_

The only thing I can think of is that python path looks a bit weird to me and I'm thinking it's potentially failing launching the debugpy instance before even launching your program.

I would double check your python interpreter path, maybe just reselect it (ctrl+shift+p >Python Select Interpreter and choose the venv interpreter).

Does just running a regular debug instance work? Something like:
```json
{
    "name": "Debug with python",
    "type": "debugpy",
    "request": "launch",
    "program": "${file}"
}
```

If that works fine then sorry I'm a bit stumped.

---

_Comment by @juanbretti on 2025-10-07 12:18_

@fionn-r , I think I have found the issue. The issue is the folders with "spaces".
My file path is like:

```
C:\Users\me\This folder\This app\src\python.exe
```
I moved everything to 
```
C:\Users\me\src\python.exe
```
And now, works with:

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug with uv",
            "type": "debugpy",
            "request": "launch",
            "python": "uv",
            "pythonArgs": [
                "run",
                "python"
            ],
            "program": "${file}"
        }
    ]
}
```

And with the default configuration.

It's very strange how bad VSCode is managing the spaces.

---

_Comment by @RafaelWO on 2025-10-08 06:31_

> These workarounds do not work with uv run script.py where the dependencies are included using uv's script dependencies, do they?
Where scripting is often where it is useful to try out things and debug. Any suggestions on how to run that from VScode easily in debug mode?

@hoegge I believe I found a way to run the VSCode debugger on scripts with inline dependencies 🙂. 

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "UV Python Script",
            "type": "debugpy",
            "request": "launch",
            "program": "${file}",
            "python": "uv",
            "pythonArgs": [
                "run", "--with-requirements", "${file}", "python"
            ]
        }
    ]
}
```

This uses the `uv run` option [`--with-requirements`](https://docs.astral.sh/uv/reference/cli/#uv-run--with-requirements) to install the inline dependencies into an ephemeral environment and then runs the corresponding script.

---

**Notes:**
First, I tried the [config from @fionn-r](https://github.com/astral-sh/uv/issues/8558#issuecomment-3354594873), which runs Python but without installing the dependencies from the inline script metadata.

Using `"pythonArgs": ["run", "--script"]` also does not work because VSCode assumes that `"python"` is a Python interpreter and passes additional arguments to the command (e.g. `-X frozen_modules=off (...)`).

---

_Comment by @wtimmerman-fitp on 2025-10-08 14:31_

@RafaelWO , exciting to see, was just struggling with that same issue with inline deps last night. I'm still finding two issues:

**UPDATE: See below, the launch.json config was slightly out of whack; also need to have a more recent version of uv to handle (uv 0.9.0 works)**

I couldn't quite get your chunk to run successfully on an example file:
```shell
(example-uv-made-venv) PS C:\Users\username\Documents\projects\example-project>  C:; cd 'C:\Users\username\Documents\projects\example-project'; & 'c:\Users\username\Documents\projects\example-project\.venv\Scripts\python.exe' 'c:\Users\username\.vscode\extensions\ms-python.debugpy-2025.10.0-win32-x64\bundled\libs\debugpy\launcher' '49986' '--' 'C:\Users\username\Documents\projects\example-project\analyses\pep723-example.py' 
error: File not found: `python`
```

this was the launch.json chunk (just added cwd item):
```json
{
      "name": "uv debug test",
      "type": "debugpy",
      "request": "launch",
      "program": "${file}",
      "python": "uv",
      "cwd": "${workspaceFolder}",
      "pythonArgs": ["run", "--with-requirements", "python", "${file}"]
    }
```

This is a minimal python file example; the debug launch was triggered with this file active in vscode.

```python
# /// script
# requires-python = ">=3.11"
# dependencies = [
#   "requests>=2.31.0"
# ]
# ///

"""
Minimal PEP 723 example script with cli args

Examples:
    uv run analyses/pep723-example.py
    uv run analyses/pep723-example.py --message "Custom message"
"""

import argparse
import requests  # not used, just to test that inline deps work


def main():
    """Main function to parse arguments and print the message."""
    parser = argparse.ArgumentParser(
        description="Minimal PEP 723 example with optional argument"
    )
    parser.add_argument(
        "--message", default="Hello, World!", help="Message to print to console"
    )
    args = parser.parse_args()

    print(f"You provided: {args.message}")


if __name__ == "__main__":
    main()

```

I'm curious if this example works on your end, or if there's just some other misconfiguration happening on my end or i'm too far behind on the version now (windows 10, uv 0.7.22, current vscode version). More broadly, i'm also having a bit of trouble getting arguments that are required at the command line to work with some of these uv-oriented launch configs. Can get debugger going fine in other repos that don't use uv, but not so much here.

---

_Comment by @RafaelWO on 2025-10-08 16:32_

@wtimmerman-fitp You mixed up two of the Python args in your `launch.json` 😉 

```diff
-      "pythonArgs": ["run", "--with-requirements", "python", "${file}"]
+      "pythonArgs": ["run", "--with-requirements", "${file}", "python"]
```

The option `--with-requirements` expects the file you want to run. Hopefully, it works if you swap the last two arguments 🙂 

---

> More broadly, i'm also having a bit of trouble getting arguments that are required at the command line to work

To specify arguments to the Python script that are parsed via `argparse`, you have to set the `"args"` key in your `lauch.json`:

```json
{
    ...
    "cwd": "${workspaceFolder}",
    "pythonArgs": ["run", "--with-requirements", "${file}", "python"],
    "args": ["--message", "Hi from VSCode"]
}
```
If you manually want to enter args upon starting the debugger, I recommend using `"args": "${command:pickArgs}"` which opens an input dialog to pass options if you start debugging. You can find more info in the [VSCode docs on Python debugging](https://code.visualstudio.com/docs/python/debugging#_args).

---

_Comment by @wtimmerman-fitp on 2025-10-08 18:46_

Oh goodness, perils of debugging for too long...

In my defense, hah, i had also tried what you put and received this other error previously:

```
error: Unexpected '"', expected '-c', '-e', '-r' or the start of a requirement at analyses\pep723-example.py:8:1
```

but, seems like an upgrade from 0.7.22 to a later version (0.9.0 in my case) did the trick.

So, big thanks, @RafaelWO ! And appreciate the insight on the additional args and your response on this item

---

_Comment by @hoegge on 2025-10-10 08:45_

Thanks a lot @RafaelWO  - it seems to work perfectly. Great!

> [@hoegge](https://github.com/hoegge) I believe I found a way to run the VSCode debugger on scripts with inline dependencies 🙂.
> 
> {
>     "version": "0.2.0",
>     "configurations": [
>         {
>             "name": "UV Python Script",
>             "type": "debugpy",
>             "request": "launch",
>             "program": "${file}",
>             "python": "uv",
>             "pythonArgs": [
>                 "run", "--with-requirements", "${file}", "python"
>             ]
>         }
>     ]
> }
> This uses the `uv run` option [`--with-requirements`](https://docs.astral.sh/uv/reference/cli/#uv-run--with-requirements) to install the inline dependencies into an ephemeral environment and then runs the corresponding script.
> 
> **Notes:** First, I tried the [config from @fionn-r](https://github.com/astral-sh/uv/issues/8558#issuecomment-3354594873), which runs Python but without installing the dependencies from the inline script metadata.
> 
> Using `"pythonArgs": ["run", "--script"]` also does not work because VSCode assumes that `"python"` is a Python interpreter and passes additional arguments to the command (e.g. `-X frozen_modules=off (...)`).



---

_Comment by @mkatkov on 2025-10-18 19:18_

> Hi all!
> 
> I am trying to see if we can start using uv to manage our codebases. Everything seems to work great, and managing dependencies is a lot easier this way! However, I can't figure out how to run a program through uv in debug mode in either VS Code or PyCharm. (e.g., `uv run src/main.py`) Does anyone have experience with running uv scripts in debug mode and can you point me in the right direction? :)
> 
> Thank you!

Hi, with a bit of less convinience it can be done:

In vscode setup debugging in the attach mode (inside launch.json - see beginning of article):
https://code.visualstudio.com/docs/python/debugging
{
  "name": "Python Debugger: Attach",
  "type": "debugpy",
  "request": "attach",
  "connect": {
    "host": "localhost",
    "port": 5678
  }
}

in uv install debugpy
uv add debugpy 
- or as dev dependency

then run 
uv run -m debugpy --wait-for-client --listen 5678 whatever/you/what/to/run.py with arguments 

The inconvenience is: yo have to restart uv command in console and start debugging in vscode, instead of clicking reload button in debugger extension. For me it is Alt-Tab Ctrl-C Ctrl-C ArrowUP Enter Alt-Tab F5, instead of just Ctrl-Shift-F5.


---

_Comment by @Ky9oss on 2025-10-31 14:52_

If you're willing to use a REPL debugger, the pdb that comes with Python 3.14 is a good choice. It adds a new `--pid` option that allows you to attach to a running process by PID. (See the docs: https://docs.python.org/3/library/pdb.html#cmdoption-pdb-p). In my case I use a PowerShell script to start the process with `uv` and attach to the Python process in the call chain. This example shows how to use it with [mitmproxy](https://github.com/mitmproxy/mitmproxy):
```powershell
# Run this script from your project's root to debug a uv project with Python 3.14's pdb

uv python pin 3.14
Start-Process -FilePath "uv.exe" -ArgumentList "run mitmproxy" -PassThru | Out-Null
Start-Sleep -Milliseconds 2000

# Process chain: uv.exe -> mitmproxy.exe -> python.exe -> python.exe
$up = Get-CimInstance Win32_Process -Filter "Name = 'uv.exe' OR Name = 'uv'" | Select-Object -ExpandProperty ProcessId
$mp = Get-CimInstance Win32_Process -Filter "ParentProcessId = $up AND Name = 'mitmproxy.exe'" | Select-Object -ExpandProperty ProcessId
$pp = Get-CimInstance Win32_Process -Filter "ParentProcessId = $mp" | Select-Object -ExpandProperty ProcessId
$pp314 = Get-CimInstance Win32_Process -Filter "ParentProcessId = $pp" | Select-Object -ExpandProperty ProcessId
python -m pdb -p $pp314
```
This works well for me — pdb in Python 3.14 has all the features I need.

---

_Comment by @konstin on 2025-12-15 17:04_

PyCharm 2025.3 now supports launching scripts such as `uv run main.py` natively:

<img width="1297" height="925" alt="Image" src="https://github.com/user-attachments/assets/7a3685cd-91ac-4b27-8552-1db0039406e7" />

<img width="1297" height="925" alt="Image" src="https://github.com/user-attachments/assets/304647e2-58dd-4211-a9ed-81677cdd37a4" />

---

_Comment by @guites on 2025-12-15 21:19_

Hey, just in case someone came here by googling "How to use uv run with vscode launch.json", I managed to get it working with the following setup:

```
{
    // Select the uv python (./venv/bin/python) as the vscode python interpreter
    "version": "0.2.0",
    "configurations": [
        {
            "name": "scanapi PokeAPI",
            "type": "debugpy",
            "request": "launch",
            "module": "scanapi",
            "args": [
                "run",
                "../examples/pokeapi/scanapi.yaml",
                "-c",
                "../examples/pokeapi/scanapi.conf",
                "-o",
                "../examples/pokeapi/scanapi-report.html"
            ]
        }
    ]
}
```

This would be equivalent to using, on the command line:

```
uv run scanapi run ../examples/pokeapi/scanapi.yaml -c ../examples/pokeapi/scanapi.conf -o ../examples/pokeapi/scanapi-report.html
```

You can change everything from `scanapi` onwards by your actual python script/program. For example flask would be

```
{
    // Select the uv python (./venv/bin/python) as the vscode python interpreter
    "version": "0.2.0",
    "configurations": [
        {
            "name": "scanapi PokeAPI",
            "type": "debugpy",
            "request": "launch",
            "module": "flask",
            "args": [
                "--app",
                "hello",
                "run",
                "--debug"
            ]
        }
    ]
}
```

---
