---
number: 15535
title: .venv always placed in home directory
type: issue
state: closed
author: ohwhydev
labels:
  - question
assignees: []
created_at: 2025-08-26T14:51:18Z
updated_at: 2025-08-26T15:02:25Z
url: https://github.com/astral-sh/uv/issues/15535
synced_at: 2026-01-10T01:25:57Z
---

# .venv always placed in home directory

---

_Issue opened by @ohwhydev on 2025-08-26 14:51_

### Question

Installed uv with Homebrew, but have tried uninstalling and using the script without any change.
  
Each time I create a new project it always uses the .venv in my home directory, rather than the project directory. Is there any reason this could be happening?

example:  
```zsh
cd ~/projects
uv init hello-world
Adding `hello-world` as member of workspace `/Users/myuser`
Initialized project `hello-world` at `/Users/myuser/projects/hello-world`

cd hello-world
uv add pytest
Using CPython 3.12.11
Creating virtual environment at: /Users/myuser/.venv
Resolved 133 packages in 3ms
Installed 5 packages in 11ms
 + iniconfig==2.1.0
 + packaging==25.0
 + pluggy==1.6.0
 + pygments==2.19.2
 + pytest==8.4.1

```

Using 'uv venv' creates a .venv folder in the local directory, but as soon as I do uv sync, uv run it uses the .venv in my home directory instead.

Any guidance appreciated, many thanks.

### Platform

macOS arm 15.6.1 - Darwin 24.6.0 arm64

### Version

uv 0.8.13 (Homebrew 2025-08-21)

---

_Label `question` added by @ohwhydev on 2025-08-26 14:51_

---

_Comment by @charliermarsh on 2025-08-26 14:52_

Very strange. Is the `UV_PROJECT_ENVIRONMENT` variable set somewhere / somehow?

---

_Comment by @ohwhydev on 2025-08-26 14:56_

Not that I can find, searched my .zshrc and .zshenv. Also echo $UV_PROJECT_ENVIRONMENT is empty. 
Scratching my head over it all day.

Only way I can get it working is to use the following at the moment:

```zsh
source .venv/bin/activate
uv run --active mycommand
```


---

_Comment by @ohwhydev on 2025-08-26 15:02_

Looking through my output above made me question what a workspace is....

I found the solution. I had a pyproject.toml file in my home directory, which was adding all new projects to that workspace.
Deleting that, not required, pyproject.toml file fixed the issue.

---

_Closed by @ohwhydev on 2025-08-26 15:02_

---
