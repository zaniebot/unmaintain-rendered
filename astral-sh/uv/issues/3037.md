```yaml
number: 3037
title: "bug: uv with vs code terminal"
type: issue
state: closed
author: TylerHillery
labels:
  - bug
assignees: []
created_at: 2024-04-15T16:55:41Z
updated_at: 2024-06-29T18:35:18Z
url: https://github.com/astral-sh/uv/issues/3037
synced_at: 2026-01-10T05:31:37Z
```

# bug: uv with vs code terminal

---

_Issue opened by @TylerHillery on 2024-04-15 16:55_

There are two bugs with using uv venv with the VS Code terminal
- I can't deactivate my environment "permission denied"
- Command prompt gets messed up with the python venv name

System Versions:
```bash
# uv --version
uv 0.1.29 (365cb16fd 2024-04-04)

# code --version
1.88.1
e170252f762678dec6ca2cc69aba1570769a5d39
arm64

# zsh --version
zsh 5.9 (x86_64-apple-darwin23.0)

# neofetch
OS: macOS 14.4 23E214 arm64
Host: Mac15,3
Kernel: 23.4.0
Shell: zsh 5.9
Terminal: iTerm2
CPU: Apple M3
GPU: Apple M3
```

## Expected Behavior 

Everything works fine if I don't use VSCode's integrated terminal.

```bash
➜  uv-vscode-mre git:(main) ✗ uv venv
Using Python 3.9.16 interpreter at: /Users/tyler/.pyenv/versions/3.9.16/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
➜  uv-vscode-mre git:(main) ✗ source .venv/bin/activate
(uv-vscode-mre) ➜  uv-vscode-mre git:(main) ✗ deactivate
➜  uv-vscode-mre git:(main) ✗
```

## What happens when using vs code terminal

After `.venv` has already been created you can tell VS Code to select python
interpreter by using the command pallet:

(CMD + Shift + P) --> Python: Select Interpreter --> Select .venv/bin/python (which was created by uv)

Now when you open up a new terminal session in vs code you get this
```bash
uv-vscode-mre➜  uv-vscode-mre git:(main) ✗
```

You can see the python venv name isn't property formatted as `(uv-vscode-mre) ➜  uv-vscode-mre git:(main) ✗`

Also when you type deactivate you get:
```
uv-vscode-mre➜  uv-vscode-mre git:(main) ✗ deactivate
zsh: permission denied: deactivate
```

## Steps to Reproduce the Error with GitHub Codespaces

1. Navigate to this [GitHub Repo](https://github.com/TylerHillery/uv-vscode-mre/tree/main)
2. Click the Green Code Button
3. Hit Codespaces
4. Create codespace on main
   1. This will take a little bit to spin up your codespace
5. Run `uv venv` in the terminal
```bash
root@codespaces-5159f8:/workspaces/uv-vscode-mre# uv venv
Using Python 3.12.3 interpreter at: /usr/local/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```
6. Open the command pallet -> Python: Select Interpreter -> 
    1. You may need to wait for the python extension to install
7. Open a new terminal session (CTRL + SHIFT + `) or the ➕ in top right corner of terminal
8. Run `deactivate`
```bash
uv-vscode-mreroot@codespaces-5159f8:/workspaces/uv-vscode-mre# deactivate
bash: /root/.vscode-remote/extensions/ms-python.python-2024.4.1/python_files/deactivate/bash/deactivate: Permission denied
```

You can see the python venv name is still formatted wrong and still getting permission denied

## Conclusion

Not sure if this is uv error or vs code error. uv works fine outside of vs code terminal and vs code terminal works fine if I use `python -m venv .venv`




---

_Comment by @charliermarsh on 2024-04-16 13:24_

Thanks for reporting! Appreciate the clear issue. (Not immediately clear to me either if it's a VS Code thing or a uv thing.)

---

_Label `bug` added by @charliermarsh on 2024-04-16 13:24_

---

_Comment by @ottaviohartman on 2024-04-23 22:26_

Looks like a VSCode issue: https://github.com/microsoft/vscode-python/issues/23195

---

_Comment by @TylerHillery on 2024-06-29 18:16_

VS Code fixed it

---

_Closed by @TylerHillery on 2024-06-29 18:16_

---

_Comment by @charliermarsh on 2024-06-29 18:35_

Let's go!

---
