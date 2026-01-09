---
number: 6672
title: "uv .venv and VS Code Jupyter Notebook: Connecting to Kernel"
type: issue
state: closed
author: wes-stone
labels:
  - question
  - windows
assignees: []
created_at: 2024-08-27T05:36:06Z
updated_at: 2025-12-30T04:01:38Z
url: https://github.com/astral-sh/uv/issues/6672
synced_at: 2026-01-07T13:12:17-06:00
---

# uv .venv and VS Code Jupyter Notebook: Connecting to Kernel

---

_Issue opened by @wes-stone on 2024-08-27 05:36_

Hello and thanks for all the great work with uv! 

I was hoping to be able to use VS Code Jupyter notebooks with just uv to install Python. Loading the .venv works fine, but when running a code block, it is stuck in an infinite loop trying to connect to a Kernel. This only happens with VS Code Jupyter Notebooks as well. If I have the web version installed but specify to use the python installed with `uv venv --python 3.10` it works fine. Would be great to not have to have a web version installed and only use uv to manage Python but understand if there's no workaround. Thanks for the help! 


```
PS C:\Users\USER\testing stuff> uv python list
cpython-3.12.5-windows-x86_64-none     <download available>
cpython-3.11.9-windows-x86_64-none     <download available>
cpython-3.10.14-windows-x86_64-none    C:\Users\USER\AppData\Roaming\uv\data\python\cpython-3.10.14-windows-x86_64-none\python.exe
cpython-3.9.19-windows-x86_64-none     <download available>
cpython-3.8.19-windows-x86_64-none     <download available>
pypy-3.7.13-windows-x86_64-none        <download available>
PS C:\Users\wesst\testing stuff> 
 *  History restored 
```

Jupyter installed as well
```
PS C:\Users\wesst\testing stuff> uv pip tree
fqdn v1.5.1
isoduration v20.11.0
└── arrow v1.3.0
    ├── python-dateutil v2.9.0.post0
    │   └── six v1.16.0
    └── types-python-dateutil v2.9.0.20240821
jsonpointer v3.0.0
jupyter v1.0.0
├── notebook v7.2.1
.....
````

```
Visual Studio Code (1.92.2, undefined, desktop)
Jupyter Extension Version: 2024.7.0.
Python Extension Version: 2024.12.3.
Pylance Extension Version: 2024.8.2.
Platform: win32 (x64).
Workspace folder ~\testing stuff, Home = c:\Users\USER
22:17:38.461 [info] Starting Kernel (Python Path: ~\testing stuff\.venv\Scripts\python.exe, Venv, 3.10.14) for '~\testing stuff\test.ipynb' (disableUI=true)
22:17:39.048 [info] Process Execution: ~\testing stuff\.venv\Scripts\python.exe -m pip list
22:17:39.060 [info] Process Execution: ~\testing stuff\.venv\Scripts\python.exe -c "import ipykernel; print(ipykernel.__version__); print("5dc3a68c-e34e-4080-9c3e-2a532b2ccb4d"); print(ipykernel.__file__)"
22:17:39.268 [info] Process Execution: ~\testing stuff\.venv\Scripts\python.exe -c "import pip;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
22:17:39.410 [warn] No interpreter with path ~\example\.venv\Scripts\python.exe found in Python API, will convert Uri path to string as Id ~\example\.venv\Scripts\python.exe
22:17:39.458 [warn] Failed to get activated env vars for ~\.virtualenvs\Documents-f1-lh8zo\Scripts\python.exe in 208ms
22:17:39.460 [error] Unable to determine site packages path for python ~\.virtualenvs\Documents-f1-lh8zo\Scripts\python.exe (Venv)
22:17:39.471 [info] Process Execution: ~\.virtualenvs\Documents-f1-lh8zo\Scripts\python.exe c:\Users\~\.vscode\extensions\ms-toolsai.jupyter-2024.7.0-win32-x64\pythonFiles\vscode_datascience_helpers\kernel_interrupt_daemon.py --ppid 14064
    > cwd: ~\.vscode\extensions\ms-toolsai.jupyter-2024.7.0-win32-x64\pythonFiles\vscode_datascience_helpers
22:17:41.868 [warn] Disposing old controller startUsingPythonInterpreter:'.jvsc74a57bd0425368e222807f297773663fdefa9d8713b24c56578a546e4a90a8565a35668f.c:\Users\~\AppData\Local\Programs\Python\Python313\python.exe.c:\Users\~\AppData\Local\Programs\Python\Python313\python.exe.-m#ipykernel_launcher' for view = 'jupyter-notebook'
22:17:41.869 [warn] Disposing old controller startUsingPythonInterpreter:'.jvsc74a57bd0425368e222807f297773663fdefa9d8713b24c56578a546e4a90a8565a35668f.c:\Users\~\AppData\Local\Programs\Python\Python313\python.exe.c:\Users\~\AppData\Local\Programs\Python\Python313\python.exe.-m#ipykernel_launcher (Interactive)' for view = 'interactive'
```

Below seems to be main issue.
```
22:17:39.458 [warn] Failed to get activated env vars for ~\.virtualenvs\Documents-f1-lh8zo\Scripts\python.exe in 208ms
22:17:39.460 [error] Unable to determine site packages path for python ~\.virtualenvs\Documents-f1-lh8zo\Scripts\python.exe (Venv)
22:17:39.471 [info] Process Execution: ~\.virtualenvs\Documents-f1-lh8zo\Scripts\python.exe c:\Users\~\.vscode\extensions\
```
 
 Python scripts run fine
 ```
PS C:\Users\USER\testing stuff> uv run .\test.py   
hello uv
```
 

---

_Comment by @zanieb on 2024-08-27 11:33_

Very weird, thanks for the report and sorry you ran into troubles here. We might need to escalate this to someone that's familiar with the Jupyter extension (cc @dhruvmanila), I have no idea why it would fail like that.

---

_Label `question` added by @zanieb on 2024-08-27 11:33_

---

_Label `windows` added by @zanieb on 2024-08-27 11:33_

---

_Comment by @dhruvmanila on 2024-08-28 06:39_

> ```
> 22:17:39.268 [info] Process Execution: ~\testing stuff\.venv\Scripts\python.exe -c "import pip;print('6af208d0-cb9c-427f-b937-ff563e17efdf')"
> 22:17:39.410 [warn] No interpreter with path ~\example\.venv\Scripts\python.exe found in Python API, will convert Uri path to string as Id ~\example\.venv\Scripts\python.exe
> ```

Huh, why is it looking for the interpreter in the "example" directory when the environment should be in "testing stuff" directory? How did you select the kernel in VS Code?



---

_Comment by @dhruvmanila on 2024-08-28 06:45_

Which kernel do you see here? Can you click on it and select the current virtual environment?

<img width="1728" alt="Screenshot 2024-08-28 at 12 14 04" src="https://github.com/user-attachments/assets/affbf2d5-3d5b-4424-a2fc-12ea694876e8">


---

_Comment by @wes-stone on 2024-08-29 06:09_

Thanks for following up and yep, just the typical set up with the kernel. 
Actually just solved it by deleting below folder in `Users\user` Seems I hadn't cleaned out thoroughly 
![image](https://github.com/user-attachments/assets/b642b59c-8d3d-4fb5-a2db-321b2194a74a)Thanks again! 



---

_Comment by @dhruvmanila on 2024-08-29 07:38_

Yeah, I guess there must be some mismatch between the (old) kernel configuration and the (new) virtual environment itself.

---

_Comment by @bjornasm on 2024-09-25 09:17_

I am not sure if it is related, but my VSCode hangs (kernel is just spinning, vscode claims it is trying to save to file) every time I add a new library using uv add and then try to import it. Have to restart vscode for it to work. I have no previous .venv in this project.

---

_Comment by @dhruvmanila on 2024-09-30 11:09_

@wesstone12 I'm assuming that the issue is resolved on your end so closing this issue.

@bjornasm It might be unrelated. Do you mind opening a new issue with the details on how to reproduce?

---

_Closed by @dhruvmanila on 2024-09-30 11:09_

---

_Comment by @scotho3 on 2025-01-17 15:57_

`uv pip install ipykernel`

fixed this for me. Adding for posterity

---

_Comment by @rram12 on 2025-07-21 22:16_

To explain more the solution of @scotho3, thanks Thomas btw, 
1- activate your virtual environment  `source /path/to/venv/bin/activate`
2- install the ipykernel `uv pip install ipykernel`
3- start vscode or jupyter server from the same command line, otherwise the kernel in question won't be selectable `code .`

---

_Comment by @meehanman on 2025-10-17 14:03_

Thanks for the help! I'm able to quickly setup a new project on MacOS with VSCode via:

1. Open a fresh directory in VSCode
2. Run `git init && uv init` to setup the project
3. Run `uv add ipykernel` to add Python Notebook Tools
4. Create a new Python Notebook via `touch notebook.ipynb`
5. Open the Notebook in VSCode and Select `Python Environments... > .venv/bin/python`
6. 🕹️ Notebook is now working with `uv` + `VSCode`

I can then add additional packages via the command-line eg. `uv add boto3` for AWS Tools and this will be added to my environment and `pyproject.toml` as well as work within my `notebook.ipynb eg:

```python
import boto3

response = boto3.client('s3').list_objects_v2(Bucket='your-bucket-name')
```

---

Bonus Quickstart
```bash
mkdir notebook && cd notebook && git init && uv init && uv add ipykernel && touch notebook.ipynb
```

---

_Comment by @JDeanThomas on 2025-12-30 04:01_

If you build a global python on a Mac via uv, VS Code will not see your env if you use tools to have a clean python and stuff everything into jupyter. You will always get an:

`Running cells with '.venv (Python 3.12.12)' requires the ipykernel package.

Install 'ipykernel' into the Python environment.`

---
