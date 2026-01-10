---
number: 2245
title: Failed to load project for workspace
type: issue
state: closed
author: amirhosseindavoody
labels:
  - question
assignees: []
created_at: 2025-12-28T09:18:49Z
updated_at: 2025-12-30T06:47:54Z
url: https://github.com/astral-sh/ty/issues/2245
synced_at: 2026-01-10T01:51:14Z
---

# Failed to load project for workspace

---

_Issue opened by @amirhosseindavoody on 2025-12-28 09:18_

### Summary

I am using `ty` extension on VScode with the following project settings

```
        "python.defaultInterpreterPath": "${workspaceFolder}/.pixi/envs/py/bin/python",
        "python.languageServer": "None",
        "ty.importStrategy": "useBundled",
        "ty.interpreter": [
            "${workspaceFolder}/.pixi/envs/py/bin/python",
        ],
        "ty.disableLanguageServices": false,
        "ty.inlayHints.variableTypes": false,
```

The python and other depenencies is installed using `pixi` in the local directory.

The extension is not able to resolve the python environment.

As a note, the same settings used to work in a different vscode workspace, but it is giving me error here. I did a couple of reload windows and one of them seemed to work but it appeared again after some time.

My development environment is that I am connecting from windows to my WSL machine. I have latest VScode, Pixi and TY extension.

Here is the `ty` logs:

```
2025-12-28 01:11:36.740 [info] Name: ty
2025-12-28 01:11:36.740 [info] Module: ty
2025-12-28 01:11:36.740 [info] Using interpreter: /home/amirhossein/mini-spice/.pixi/envs/py/bin/python
2025-12-28 01:11:38.260 [info] Initialization options: {}
2025-12-28 01:11:38.260 [info] Using bundled executable: /home/amirhossein/.vscode-server/extensions/astral-sh.ty-2025.76.0-linux-x64/bundled/libs/bin/ty
2025-12-28 01:11:38.265 [info] Found executable at /home/amirhossein/.vscode-server/extensions/astral-sh.ty-2025.76.0-linux-x64/bundled/libs/bin/ty
2025-12-28 01:11:38.265 [info] Server run command: /home/amirhossein/.vscode-server/extensions/astral-sh.ty-2025.76.0-linux-x64/bundled/libs/bin/ty server
2025-12-28 01:11:38.266 [info] Server: Start requested.
```

Here is the `ty Language Server` logs:

```
2025-12-28 01:11:38.288542668  INFO Version: 0.0.6
2025-12-28 01:11:38.452715741  INFO Defaulting to python-platform `linux`
2025-12-28 01:11:38.452918854 ERROR Failed to create project for workspace `file:///home/amirhossein/mini-spice`: Failed to discover the site-packages directory: Invalid selected interpreter in your editor `/usr`: Could not find a `site-packages` directory for this Python installation/executable. Falling back to default settings
2025-12-28 01:11:38.453080736  INFO Defaulting to python-platform `linux`
2025-12-28 01:11:38.453421130  INFO Python version: Python 3.14, platform: linux
```


### Version

0.0.6

---

_Comment by @MichaReiser on 2025-12-29 10:43_

Hi. 

I'm sorry you're running into this and thanks for sharing all the logs. They're very helfpul. 

Can you double check what interpreter you've selected in VS Code and if it is correct. To do so, open the command pallete (Cmd+Shift+P), "Python: Select interpreter", select the environment for this workspace.



---

_Label `question` added by @MichaReiser on 2025-12-29 10:43_

---

_Comment by @dhruvmanila on 2025-12-30 05:46_

(This might be unrelated but the only purpose for the [`ty.interpreter`](https://docs.astral.sh/ty/reference/editor-settings/#interpreter) is to provide the Python interpreter to use for finding the `ty` executable and it's not the same as [`ty.environment.python`](https://docs.astral.sh/ty/reference/configuration/#python))

---

_Comment by @amirhosseindavoody on 2025-12-30 06:45_

Thanks for the tip. Adding `ty.toml` file with the following content fixed the issue.

```toml
[environment]
python = "./.pixi/envs/py/bin/python"
```

---

_Closed by @amirhosseindavoody on 2025-12-30 06:45_

---

_Comment by @amirhosseindavoody on 2025-12-30 06:47_

> Hi.
> 
> I'm sorry you're running into this and thanks for sharing all the logs. They're very helfpul.
> 
> Can you double check what interpreter you've selected in VS Code and if it is correct. To do so, open the command pallete (Cmd+Shift+P), "Python: Select interpreter", select the environment for this workspace.

Also, before adding the ty.toml file, I tried selecting the correct python Interpreter, but it didn't help.

---
