---
number: 2410
title: "Server: Mono-repo support"
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-05-25T11:19:49Z
updated_at: 2026-01-09T10:24:54Z
url: https://github.com/astral-sh/ty/issues/2410
synced_at: 2026-01-10T01:48:23Z
---

# Server: Mono-repo support

---

_Issue opened by @MichaReiser on 2025-05-25 11:19_

It's not uncommon to have a single repository that contains both the frontend and backend (Python) code. As a user, I want to be able to have the ty configuration in the backend folder. See

https://github.com/astral-sh/ty-vscode/issues/40
https://github.com/astral-sh/ty-vscode/issues/17

We could

* Scan for configurations in sub-directories if the root folder has no explicit `ty` configuration
* Add an option

---

_Comment by @kkpattern on 2025-05-27 12:05_

Add an option would be great. Currently we're using the following setting with mypy:

```json
		"mypy-type-checker.args": [
			"--config-file=${workspaceFolder}/client/pyproject.toml"
		],
```

---

_Added to milestone `Beta` by @MichaReiser on 2025-05-28 12:08_

---

_Comment by @kkpattern on 2025-07-03 02:17_

`ty` itself now has `--project` and `--config-file` parameters. If we add `ty.project` and `ty.configFile` to the ty-vscode settings, this should be largely solved. We can create different vscode workspace for different project in the monorepo, or use [multi-root workspace](https://code.visualstudio.com/docs/editing/workspaces/multi-root-workspaces), then put the corresponding `project` and `configFile` in the workspace settings.

---

_Comment by @dhruvmanila on 2025-07-03 05:25_

Thanks for the suggestions! Micha is planning to look into the entire spectrum (multiple workspaces, multiple ty projects in a single workspace, multi-root workspaces, mono-repository support) this month. It's a bit more complicated than it looks and we need to spend some time thinking through the design first.

---

_Comment by @kkpattern on 2025-07-03 06:15_

Totally get it. Since full monorepo support is complicated, maybe we can add the `ty.configFile` or even `ty.args` setting first, so we can start to test it in our monorepo. We know ty isn't production ready yet, but ty is so awesome that we can't help but try it out. ;)

---

_Comment by @bluecoconut on 2025-07-04 02:57_

I'm not sure if this is related, but I just found out about `projects` via this extension: https://github.com/microsoft/vscode-python-environments which seems like a great way to manage which interpreter is selected per file and related to the greater issue of python mono-repos in vscode.

By right-clicking on the folder in the Explorer fold, I was able to add the folder as a "project". After this, `ruff` handled this very well, but `ty` didn't, which I suspect is known so I'm anecdotally reporting that experience here.

For now, I have a single `ty.toml` at the top of my project and I have to manually switch it to the project directory I'm working in.

I'm very excited for this feature to get supported!

---

_Removed from milestone `Beta` by @MichaReiser on 2025-08-15 12:01_

---

_Comment by @faroukcharkas on 2025-09-29 17:57_

I was running into persistent `ty(unresolved-import)` linting errors in my IDE (Cursor / VSCode) when running in a monorepo, even though running `uv ty check` in the CLI did not show those errors.

My project structure looked like this:
```
server/
  src/
    modules/
      module_a/
  app.py   # Error said it couldn’t find module_a
client/
README.md
```

### What I had before

My VSCode `settings.json`:
```json
{
  "ty.importStrategy": "fromEnvironment",
  "ty.interpreter": ["${workspaceFolder}/server/.venv/bin/python"],
  "ty.diagnosticMode": "workspace",
  "ty.path": ["${workspaceFolder}/server/.venv/ty"]
}
```

My `pyproject.toml`:
```toml
[tool.ty.environment]
root = ["."]
python = "./.venv"
```

With this setup:

* **CLI (`uv ty check`) worked fine.**
* **IDE still showed unresolved imports.**

### What fixed it

I added a `ty.toml` at the **project root** (same level as `server/` and `client/`) with:

```toml
[environment]
root = ["./server"]
```

After that, the IDE immediately resolved imports correctly. This was super frustrating to debug, so hopefully this helps others who hit the same issue.

(Used AI to clean up my thought process)

---

_Comment by @akvadrako on 2025-12-07 09:27_

Wouldn't it make sense for ty to look for the closest pyproject.yaml file and use that? Then it would work without configuration. So for example:

```
/pyproject.yaml
/client/
/client/pyproject.yaml # ← this root would be used
/client/src/file.py    # ← for this source
```

---

_Comment by @MichaReiser on 2025-12-07 10:30_

ty already uses the closest pyproject.toml to where you run ty from. What's currently not supported is that ty uses the closest pyproject.toml closest to the checked file. Changing this isn't trivial, depending on what settings you want to override but is something we want to explore

---

_Comment by @raylu on 2025-12-12 20:02_

is there any workaround for this right now? it doesn't seem like there's an easy way to set the cwd of ty

but even doing it the hard way doesn't seem to work
I tried setting `ty.path` to `${workspaceFolder}/scripts/ty-vsc` which is
```sh
#!/usr/bin/env sh
cd subdir
exec venv/bin/ty "$@"
```
effectively hardcoding our `python.defaultInterpreterPath`
but it still seems to be ignoring `subdir/pyproject.toml` (I have `invalid-argument-type = 'ignore'` but it complains about `invalid-argument-type`), even though the `PWD` is set to subdir correctly (at least according to `ps eww`)
`ty server` also doesn't take `--config-file`

---

_Comment by @kkpattern on 2025-12-13 11:35_

> is there any workaround for this right now? it doesn't seem like there's an easy way to set the cwd of ty

What we are doing now is creating a multi-root workspace and putting the target sub-project dir as the first one, then the root dir as the second one. ty-vscode currently will always pick the first workspace. The downside is that you need to create a separate workspace file for each Python sub-project. It's acceptable for us now.

---

_Comment by @Gankra on 2025-12-13 13:06_

The latest release (on dec 12th) came with some improvements to how we resolve imports in monorepos. Whether this improves your usecase depends on which specific issues you had.

---

_Comment by @astrojuanlu on 2025-12-17 10:13_

Came here to report the same thing. My use case is: a single workspace with different folders.

Even though my folders have a Python interpreter configured (a different one for each, coming from a `.venv`), I see this in the ty extension logs:

```
2025-12-17 11:04:46.990 [info] Python extension loaded
2025-12-17 11:04:47.090 [error] Python interpreter missing:
[Option 1] Select Python interpreter using the ms-python.python.
[Option 2] Set an interpreter using "ty.interpreter" setting.
Please use Python 3.8 or greater.
```

You can seemingly configure `ty.interpreter` at the Workspace level, but not at the Folder level. If I set it to `${workspaceFolder}/.venv/bin/python`, I see a popup with this content:

```
Multiple workspaces are not yet supported, using the first workspace: ...
```

---

_Comment by @AnirudhKonduru on 2025-12-17 19:44_

It'll would also to be nice to support [PEP-723](https://peps.python.org/pep-0723/) scripts, and use the same interpreter and environment that `uv run` would execute the script with, disregarding the project's interpreter and environment if it exists.

---

_Comment by @MichaReiser on 2025-12-18 08:58_

@AnirudhKonduru see https://github.com/astral-sh/ty/issues/691

---

_Removed from milestone `Stable` by @MichaReiser on 2026-01-09 09:18_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-09 09:18_

---

_Renamed from "Mono-repo support" to "Server: Mono-repo support" by @MichaReiser on 2026-01-09 09:21_

---

_Comment by @MichaReiser on 2026-01-09 09:22_

See https://github.com/astral-sh/ty/issues/1554 for more discussions on design and a prototype of one approach.

---

_Label `server` added by @MichaReiser on 2026-01-09 10:24_

---
