---
number: 8706
title: vscode trying but failing to use my uv venv
type: issue
state: closed
author: MaxPowerWasTaken
labels:
  - needs-mre
assignees: []
created_at: 2024-10-30T18:55:36Z
updated_at: 2025-12-04T23:06:18Z
url: https://github.com/astral-sh/uv/issues/8706
synced_at: 2026-01-10T01:24:31Z
---

# vscode trying but failing to use my uv venv

---

_Issue opened by @MaxPowerWasTaken on 2024-10-30 18:55_

I'm pretty sure this is a bug with vscode's integrated terminal, not uv, but after getting no answers from questions posted [at stack overflow](https://stackoverflow.com/questions/79037945/vscode-trying-but-failing-to-use-my-uv-venv) and on [vscode's github](https://github.com/microsoft/vscode-python/issues/24780) a month ago, figured I'd take a flier on seeing if anyone on uv's side has any advice? I'd love to use uv but this is currently my blocker:

I have the following code in my `main.py`:
```
import pymupdf

print(f"pymupdf version is: {pymupdf.__version__}")
```

I can create a python3.12 environment with pymupdf installed and run it from my (kubuntu24.04) shell without issues, with the following commands (showing shell log after each >command):
```
>uv --version
uv 0.4.17

>uv init --no-pin-python
Initialized project `askliz2`

>uv venv --python 3.12.6 
Using CPython 3.12.6
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

>source .venv/bin/activate 
askliz2>uv add pymupdf 
Resolved 3 packages in 281ms
Installed 2 packages in 11ms
 + pymupdf==1.24.10
 + pymupdfb==1.24.10

askliz2>python main.py
pymupdf version is: 1.24.10
```

However, when I launch vscode from this folder with my uv venv activated with code ., and then highlight the line import pymupdf and hit shift+enter to run it in the integrated terminal, I get the following vscode integrated terminal output:
```
/home/max/askliz2/.venv/bin/python 
Python 3.12.6 (main, Sep  9 2024, 22:11:19) [Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
]633;E;exit()]633;D;0]633;A>>> ]633;B]633;Cimport pymupdf
]633;E;import pymupdf]633;D;0]633;A>>> ]633;B]633;C
```

Things I've tried so far:
* upgrading uv version
* hosing and re-installing vscode
* installing different python versions (e.g. 3.12.0, 3.12.6, 3.9)
* right-clicking to `clear terminal` and `kill terminal` and then trying again

When I use the command pallet to select python interpreter, it shows me it's trying to use Python 3.12.6('.venv') ./.venv/bin/python, as it should be, and as the vscode integrated logs above already indicate.


VS Code version: Code 1.93.1 (38c31bc77e0dd6ae88a4e9cc93428cc27a56ba40, 2024-09-11T17:20:05.685Z)
OS version: Linux x64 6.10.6-061006-generic
 

---

_Comment by @zanieb on 2024-10-30 19:55_

Hm I can't reproduce this, i.e., with Run Python -> Run selected line

```
❯ /Users/zb/workspace/uv/example/.venv/bin/python
Python 3.11.10 (main, Sep  7 2024, 01:03:31) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import subprocess
>>> 
```

---

_Comment by @zanieb on 2024-10-30 19:56_

What shell are you using? Is it any different if you create a virtual environment without uv?

---

_Label `needs-mre` added by @zanieb on 2024-10-30 19:56_

---

_Comment by @MaxPowerWasTaken on 2024-10-30 20:15_

thanks for the quick response @zanieb! interesting you can't reproduce. to answer your two questions:
1. I used warp and konsole (kubuntu built-in terminal) shells to create the venv with uv and then launch vscode (`code .`) from. 
2. yeah vscode/run-selection worked as expected when I created my venv with `python -m venv myenv`, from both konsole and warp shells.

---

_Comment by @zanieb on 2024-10-30 20:21_

Are you using the system or managed Python versions for your environments? Do you see a difference between those? (You can toggle with `uv venv --python-preference <only-managed | only-system>`)

What does

```
echo $SHELL
echo $TERM
```

show?

---

_Comment by @zanieb on 2024-10-30 20:24_

Have you tried toggling `Terminal › Integrated › Shell Integration: Decorations Enabled` or `Terminal › Integrated › Shell Integration: Enabled`? Do you have any custom terminal settings in your `settings.json`?

---

_Comment by @MaxPowerWasTaken on 2024-11-04 03:00_

Hi @zanieb , sorry for the delay in replying. I started up a fresh new project/folder and am still getting this issue, so want to document the exact set of commands I ran before jumping into vscode and hitting this issue again. then I'll post the output to the commands you asked for...

```
mkdir askliz_frontend
cd askliz_frontend
uv init
```

Then added some packages:
```
 uv add google-generativeai==0.8.3
 uv add streamlit==1.39.0
uv add lancedb==0.15.0
uv tool install ruff
```
then seeing with `ls -a`,  that a `.venv` has already been created, I ran `source .venv/bin/activate` and `code .` to hop into vscode, then I highlighted:
```
def main():
    print("Hello from askliz-frontend!")
```
...which came populated in `hello.py`, hit shift+enter to run it in the integrated terminal, and saw the same terminal output as before:
```
 ~/askliz_frontend  main ?6  /home/max/askliz_frontend/.venv/bin/python                                      ✔  askliz_frontend Py  20:14:49 
Python 3.12.6 (main, Sep  9 2024, 22:11:19) [Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
]633;E;exit()]633;D;0]633;A>>> ]633;B]633;Cdef main():
...     print("Hello from askliz-frontend!")
... 
``` 
...now to answer your questions:

What does `echo $SHELL` show?
```
/usr/bin/zsh
```

What does `echo $TERM` show?
```
xterm-256color
```

Any shell-integration related settings in my settings.json? I see these:
```
    "terminal.integrated.shellIntegration.enabled": false,
    "python.terminal.shellIntegration.enabled": true
```
...I then tried reversing the first from `false` to `true` and restarting vscode, but still got the same weird integrated-terminal behavior as before. And then I changed the second from `true` to `false` and restarted vscode again, and again still observed the weird behavior.

...On `Terminal › Integrated › Shell Integration: Decorations Enabled`, I'm not quite sure what you mean. I don't have anything related to that in my custom/user settings.json. When I search for 'decorations' in my default settings.json, here's the entry I see that I think is most relevant to what you're asking about:
```
	// When shell integration is enabled, adds a decoration for each command.
	//  - both: Show decorations in the gutter (left) and overview ruler (right)
	//  - gutter: Show gutter decorations to the left of the terminal
	//  - overviewRuler: Show overview ruler decorations to the right of the terminal
	//  - never: Do not show decorations
	"terminal.integrated.shellIntegration.decorationsEnabled": "both",
```

Thanks again for looking into this for me!

---

_Comment by @RedHeartSecretMan on 2024-12-27 01:54_

I had the same problem migrating from the conda and pip habit. Setting up python plug-ins in vs code The following two configurations may work:

Python: Venv Path

Python: Venv Folders

You can try.


---

_Comment by @sullicsullic on 2025-01-02 06:09_

> I had the same problem migrating from the conda and pip habit. Setting up python plug-ins in vs code The following two configurations may work:
> 
> Python: Venv Path
> 
> Python: Venv Folders
> 
> You can try.

yes, it works.

---

_Comment by @zanieb on 2025-01-02 08:56_

Sorry I'm not sure there's anything more we can do here.

---

_Comment by @MaxPowerWasTaken on 2025-01-02 15:46_

@zanieb no worries, appreciate the effort

---

_Closed by @zanieb on 2025-01-02 17:38_

---

_Comment by @AdrKacz on 2025-02-16 12:14_

> I had the same problem migrating from the conda and pip habit. Setting up python plug-ins in vs code The following two configurations may work:
> 
> Python: Venv Path
> 
> Python: Venv Folders
> 
> You can try.

@RedHeartSecretMan where do you set this? I'm facing the same issue as @MaxPowerWasTaken. Thank you 👍 

---

_Comment by @PacoBahena on 2025-04-02 20:03_

when you select the interpreter, in vscode make sure you pass down the interpreter path manually, because if passing via ui (finder/windows explorer) vscode will set the symlink to uv system python causing the link to fail.

---

_Comment by @muhammadyaseen on 2025-04-07 14:22_

> when you select the interpreter, in vscode make sure you pass down the interpreter path manually, because if passing via ui (finder/windows explorer) vscode will set the symlink to uv system python causing the link to fail.

This just saved me after 30 mins of fiddling with different settings. Thanks!

---

_Comment by @marty0678 on 2025-04-11 03:00_

Tried migrating to uv today and got the same thing. Even setting the path manually, I can't get VsCode to activate it. Will just go back to poetry for now, unfortunately. 

---

_Comment by @mustiem on 2025-07-14 19:21_

@marty0678 this is how I solved it. 

1. Opened my .venv folder in vscode, right clicked the `bin` folder and pressed `Copy Path`. 
2. Cmd + Shift + P - python: select interpreter
3. Enter Interpreter path
4. Paste path and hit enter


---

_Referenced in [astral-sh/uv#9637](../../astral-sh/uv/issues/9637.md) on 2025-12-04 21:46_

---

_Comment by @ajstair on 2025-12-04 23:06_

> [@marty0678](https://github.com/marty0678) this is how I solved it.
> 
>     1. Opened my .venv folder in vscode, right clicked the `bin` folder and pressed `Copy Path`.
> 
>     2. Cmd + Shift + P - python: select interpreter
> 
>     3. Enter Interpreter path
> 
>     4. Paste path and hit enter

As far as I can tell, this solution does not work for UV users with a monorepo setup. 

---
