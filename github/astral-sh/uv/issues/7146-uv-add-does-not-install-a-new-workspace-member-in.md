---
number: 7146
title: "`uv add` does not install a new workspace member in an editable workspace (appears to mistakenly put it in a `.venv` and miss the active conda env)"
type: issue
state: closed
author: lmmx
labels:
  - question
assignees: []
created_at: 2024-09-06T22:55:29Z
updated_at: 2024-09-07T00:13:20Z
url: https://github.com/astral-sh/uv/issues/7146
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv add` does not install a new workspace member in an editable workspace (appears to mistakenly put it in a `.venv` and miss the active conda env)

---

_Issue opened by @lmmx on 2024-09-06 22:55_

I encountered this on the 25th of August and reported it, but didn't catch Charlie's reply asking for further details on Discord. I said that if it happened again I'd open a bug ticket, so here I am :-)

I was in the terminal at the time (reproducing a different bug, now resolved for my needs thanks to the Discord chat), so I have the bug red-handed in terminal logs:

Firstly I made a new directory, and ran `uv init --app --package` which Charlie said was the best way to set a workspace member up so uv understands it's a package that should be build as editable:

```sh
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ cd packages/                                                                      (uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified/packages $ mkdir foo                                                           
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified/packages $ cd foo                                                              
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified/packages/foo $ uv init --app --package
Project `foo` is already a member of workspace `/home/louis/lab/uv/editable-ws-dep-bug-simplified`
Initialized project `foo` 
```

So far so good.

I wanted to copy the main module over from the existing workspace member package, from `packages/demo_cli/src/demo_cli/main.py`:

```sh
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ cp packages/demo_cli/src/demo_cli/main.py packages/foo/src/foo/main.py                                                                                              
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ vim packages/demo_cli/pyproject.toml                                                                                                                                
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ vim packages/foo/src/foo/                                                                                                                                           
__init__.py  main.py                                                                                                                                                                                                                          
```

I wondered if `pip list` would show it as installed now:

```sh
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ pip list                                                                                                                                                            
Package               Version Editable project location                                                                                                                                                                                       
--------------------- ------- -------------------------------------------------------------------                                                                                                                                             
demo-cli              0.1.0   /home/louis/lab/uv/editable-ws-dep-bug-simplified/packages/demo_cli                                                                                                                                             
pip                   24.2                                                                                                                                                                                                                    
setuptools            72.1.0                                                                                                                                                                                                                  
wheel                 0.43.0                                                                                                                                                                                                                  
ws_editable_dep_repro 0.0.1   /home/louis/lab/uv/editable-ws-dep-bug-simplified                                                                                                                                                               
```

The new workspace member `foo` was not there! :open_mouth: But then I recalled the thread ([1277304269845958719][thread])

> uv add does resolve and install into the environment. Are you seeing otherwise?

[thread]: https://discord.com/channels/1039017663004942429/1080933313704886385/threads/1277304269845958719

I think here I reviewed the root workspace `pyproject.toml` and saw that foo hadn't been added as a root workspace dependency: "right, so I just need to `uv add foo` I thought...

```sh
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ vim pyproject.toml                                                                                                                                                  
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ uv add foo                                                                                                                                                          
Using Python 3.12.4 interpreter at: /home/louis/miniconda3/envs/uv-ws-edit-dep-repro/bin/python3                                                                                                                                              
Creating virtualenv at: .venv                                                                                                                                                                                                                 
Resolved 3 packages in 4ms                                                                                                                                                                                                                    
   Built foo @ file:///home/louis/lab/uv/editable-ws-dep-bug-simplified/packages/foo                                                                                                                                                          
   Built ws-editable-dep-repro @ file:///home/louis/lab/uv/editable-ws-dep-bug-simplified                                                                                                                                                     
Prepared 2 packages in 271ms                                                                                                                                                                                                                  
Installed 3 packages in 1ms                                                                                                                                                                                                                   
 + demo-cli==0.1.0 (from file:///home/louis/lab/uv/editable-ws-dep-bug-simplified/packages/demo_cli)                                                                                                                                          
 + foo==0.1.0 (from file:///home/louis/lab/uv/editable-ws-dep-bug-simplified/packages/foo)                                                                                                                                                    
 + ws-editable-dep-repro==0.0.1 (from file:///home/louis/lab/uv/editable-ws-dep-bug-simplified)                                                                                                                                               
```

It looks like it **made a `.venv`** and ignored the conda env I was in? :monocle_face: Maybe that's the source of this bug

Maybe it is prioritising a `.venv` install for a new workspace member...

I didn't notice that at the time and ran `pip list` again, and was surprised to see `foo` still not in the list of installed packages:

```
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ pip list                                                                                                                                                            
Package               Version Editable project location                                                                                                                                                                                       
--------------------- ------- -------------------------------------------------------------------                                                                                                                                             
demo-cli              0.1.0   /home/louis/lab/uv/editable-ws-dep-bug-simplified/packages/demo_cli                                                                                                                                             
pip                   24.2
setuptools            72.1.0
wheel                 0.43.0
ws_editable_dep_repro 0.0.1   /home/louis/lab/uv/editable-ws-dep-bug-simplified
```

I then opened a Python REPL and confirmed

```sh
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ python -q
>>> import foo
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'foo'
>>> 
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ 
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ view pyproject.toml 
```

I then thought maybe I need to reinstall the workspace with `uv pip install -e .`:

```sh
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ uv pip install -e .
Resolved 3 packages in 3ms
Uninstalled 1 package in 0.48ms
Installed 2 packages in 3ms
 + foo==0.1.0 (from file:///home/louis/lab/uv/editable-ws-dep-bug-simplified/packages/foo)
 ~ ws-editable-dep-repro==0.0.1 (from file:///home/louis/lab/uv/editable-ws-dep-bug-simplified)
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ pip list
Package               Version Editable project location
--------------------- ------- -------------------------------------------------------------------
demo-cli              0.1.0   /home/louis/lab/uv/editable-ws-dep-bug-simplified/packages/demo_cli
foo                   0.1.0   /home/louis/lab/uv/editable-ws-dep-bug-simplified/packages/foo
pip                   24.2
setuptools            72.1.0
wheel                 0.43.0
ws_editable_dep_repro 0.0.1   /home/louis/lab/uv/editable-ws-dep-bug-simplified
```

I then finally got `foo`!

Reviewing these terminal logs I think the bug occurred here:

```sh
(uv-ws-edit-dep-repro) louis 🚶 ~/lab/uv/editable-ws-dep-bug-simplified $ uv add foo                                                                                                                                                          
Using Python 3.12.4 interpreter at: /home/louis/miniconda3/envs/uv-ws-edit-dep-repro/bin/python3                                                                                                                                              
Creating virtualenv at: .venv
```

The context for this was a minimal repro already: [repo link](https://github.com/lmmx/editable-ws-dep-bug-simplified/tree/fb3fd3aba04ee636df3286c2219f8647cc27d7ac) (at `fb3fd3a`)

- uv version 0.4.6
- platform: Linux Mint 21.3

---

_Renamed from "`uv add` does not install a new workspace member in an editable workspace" to "`uv add` does not install a new workspace member in an editable workspace (appears to mistakenly put it in a `.venv` and miss the active conda env)" by @lmmx on 2024-09-06 22:55_

---

_Comment by @charliermarsh on 2024-09-06 23:22_

So, I think this is intended, in that the "project APIs" (`uv sync`, `uv run`, `uv lock`, `uv add`) always operate in a virtual environment located at `.venv` within the project root. That's an intentional decision and differs from the `uv pip` APIs, where you can kind of do whatever you want.

You _can_ configure the path via `UV_PROJECT_ENVIRONMENT` (https://docs.astral.sh/uv/concepts/projects/#configuring-the-project-environment-path), but that's mostly intended for deployments.

In general, we don't recommend using the project APIs if you need to integrate with an existing workflow or environment (like with Conda) -- in that case, you're generally better off with the `uv pip` APIs.

Does that make sense?


---

_Label `question` added by @charliermarsh on 2024-09-06 23:22_

---

_Comment by @lmmx on 2024-09-07 00:13_

Hmmm yeah that makes sense, so you're saying workspaces prefer to be managed in their own `.venv`... If that's the standard approach I'll give it a go!

I'm only really tied to conda when I'm using non-Python env deps like CUDA or ffmpeg, I'll try out different options, thanks :-)

The other alternative here is to just to be aware of this default behaviour and run `uv pip install -e packages/*` (etc), which doesn't unduly bother me TBH.

(FWIW I tried changing all the build backends to uv's default one `hatchling` and workspace install failed said "error: Failed to prepare distributions" and suggested that I'd included a directory called `uv-ws`, which I hadn't, so I reverted to PDM build backend which I know is robust)

Closing this thanks for covering it

---

_Closed by @lmmx on 2024-09-07 00:13_

---
