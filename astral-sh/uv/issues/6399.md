```yaml
number: 6399
title: pyright language server crashes when run from inside venv created by uv
type: issue
state: closed
author: DetachHead
labels:
  - windows
assignees: []
created_at: 2024-08-22T01:55:15Z
updated_at: 2025-11-09T04:17:47Z
url: https://github.com/astral-sh/uv/issues/6399
synced_at: 2026-01-10T03:23:52Z
```

# pyright language server crashes when run from inside venv created by uv

---

_Issue opened by @DetachHead on 2024-08-22 01:55_

i'm not sure what's causing this, but for some reason all of my language servers are crashing with strange errors when running from a venv created by uv (tested with both ruff and basedpyright language servers).

# to reproduce

1. install uv using [pyprojectx](https://pyprojectx.github.io):
    ```ps1
    Invoke-WebRequest https://github.com/pyprojectx/pyprojectx/releases/latest/download/wrappers.zip -OutFile wrappers.zip; Expand-Archive -Force -Path wrappers.zip -DestinationPath .; Remove-Item -Path wrappers.zip
    ./pw --add uv
    ```
2. `./pw uv add ruff`
3. `./pw uv add basedpyright==1.17.0` (i added a workaround in a later release, see comment below)
4. install the ruff and basedpyright vscode extensions
5. set the python interpreter (<kbd>F1</kbd> > "Python: Select Interpreter" > ".venv")
6. restart vscode (probably not necessary but just in case)
7. click the Output tab in the terminal and select "Ruff" from the dropdown

    ```
    2024-08-22 11:34:42.274 [info] Using interpreter: c:\Users\user\project\.venv\Scripts\python.exe
    2024-08-22 11:34:42.274 [error] Error while trying to find the Ruff binary: Error: No pyvenv.cfg file
    
    2024-08-22 11:34:43.149 [info] Falling back to bundled executable: c:\Users\user\.vscode\extensions\charliermarsh.ruff-2024.42.0-win32-x64\bundled\libs\bin\ruff.exe
    ```

    my venv does include the `pyvenv.cfg` file so i don't know why that error is occurring:
    
    ![image](https://github.com/user-attachments/assets/44c71233-8dc7-4f20-b4eb-58ae7e3f7b2d)
8. check the basedpyright output:
    ```
    [Error - 12:04:17 PM] Server process exited with code 3221225477.
    [Error - 12:04:17 PM] Server initialization failed.
      Message: Pending response rejected since connection got disposed
      Code: -32097 
    [Info  - 12:04:17 PM] Connection to server got closed. Server will restart.
    ```

# environment info

windows 11, python 3.12, vscode 1.92.2, uv 0.3.1, ruff 0.6.1

---

_Comment by @charliermarsh on 2024-08-22 03:35_

Thanks, that's really strange. @dhruvmanila, would you have any ideas here?

---

_Label `windows` added by @charliermarsh on 2024-08-22 03:35_

---

_Comment by @dhruvmanila on 2024-08-22 03:42_

Interesting, not sure. I'd need to look

---

_Comment by @dhruvmanila on 2024-08-22 03:44_

Installing `pyprojectx` requires a `pyproject.toml` file. What does that look like? Does it need to be specific to `pyprojectx`?

---

_Comment by @DetachHead on 2024-08-22 03:54_

sorry i shouldve updated the issue. pyprojectx is not required to reproduce the issue, i included those steps just in case but a colleague was able to reproduce the issue by installing uv normally.

i should also mention that i can't reproduce those ruff errors anymore, only the basedpyright errors.

---

_Comment by @dhruvmanila on 2024-08-22 04:14_

> sorry i shouldve updated the issue. pyprojectx is not required to reproduce the issue, i included those steps just in case but a colleague was able to reproduce the issue by installing uv normally.

So, should the steps be the following?
1. Install uv
2. `uv init`
3. `uv add ruff`
4. `uv add basedpyright`
5. Install the "basedpyright" VS Code extension, disable Pylance extension
6. Open VS Code, select the virtual environment

It's working fine on MacOS at least, let me (or ask someone) try it on a Windows machine.

---

_Comment by @DetachHead on 2024-08-22 04:25_

hmm this is very odd, i can't seem to reproduce the issue anymore. i'll just close this issue for now and i will investigate further and reopen it once i have more info. sorry to waste your time

---

_Closed by @DetachHead on 2024-08-22 04:25_

---

_Comment by @DetachHead on 2024-08-22 04:40_

nevermind it does still happen with those steps, i just forgot to restart vscode so it was using the version of basedpyright bundled with the extension instead of the one in the venv.



---

_Reopened by @DetachHead on 2024-08-22 04:40_

---

_Comment by @DetachHead on 2024-08-22 05:31_

also reproduced with the [pyright pypi package](https://pypi.org/project/pyright/)
1. `uv add pyright`
2. `uv run pyright --version` to trigger the wrapper thing to install the npm package (might not be necessary)
3. install [generic lsp client](https://marketplace.visualstudio.com/items?itemName=llllvvuu.llllvvuu-glspc) (3rd party extension is required because [pyright/pylance doesn't have a way to run the lsp from your python environment](https://github.com/microsoft/pyright/pull/6644))
4. add the following to your `.vscode/settings.json`:
    ```json
    {
        "glspc.languageId": "python",
        "glspc.serverCommand": ".venv\\Scripts\\pyright-langserver.exe",
        "glspc.serverCommandArguments": [
            "--stdio"
        ]
    }
    ```
5. restart vscode
6. open a python file
7. go to the Output tab and select "glspc" from the dropdown:
    ```
    starting glspc...
    Server process exited with code 3221225477 and signal null
    Server process exited with code 3221225477 and signal null
    Server process exited with code 3221225477 and signal null
    Server process exited with code 3221225477 and signal null
    Server process exited with code 3221225477 and signal null
    ```

---

_Comment by @DetachHead on 2024-08-24 03:58_

so i tried to investigate this further on my end and came to the conclusion that it's something to do with the `basedpyright-langserver.exe` wrapper binary in `.venv/Scripts`. after installing basdepyright with uv, replacing the one created by uv with the one created by pip makes it work again.

i also found that passing `shell: true` when launching the language server from the vscode extension fixes it. https://github.com/DetachHead/basedpyright/pull/613

---

_Renamed from "language servers crash when run from inside venv created by uv" to "pyright language server crashes when run from inside venv created by uv" by @DetachHead on 2024-08-24 11:49_

---

_Comment by @DetachHead on 2025-11-09 04:17_

looks like this was the same issue as #6464 which has been fixed

---

_Closed by @DetachHead on 2025-11-09 04:17_

---
