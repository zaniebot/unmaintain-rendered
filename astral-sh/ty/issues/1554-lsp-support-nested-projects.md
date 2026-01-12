```yaml
number: 1554
title: "LSP: Support nested projects"
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-11-14T13:38:35Z
updated_at: 2026-01-09T09:22:14Z
url: https://github.com/astral-sh/ty/issues/1554
synced_at: 2026-01-12T15:54:25Z
```

# LSP: Support nested projects

---

_@MichaReiser_

When opening a folder that contains nested ty projects, ty should use the correct configuration for each nested project rathern than using the outer most configuration.


```
- parser/
  - src/parser
    - lib.py
  - ty.toml
- linter
  - src/linter
    - lib.py
  - ty.toml  
```

ty should use the `parser/ty.toml` configuration for all files in `parser/` and the `linter/ty.toml` configuration for all files in `linter`. 



---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 13:38_

---

_Label `server` added by @MichaReiser on 2025-11-14 13:38_

---

_Comment by @carljm on 2025-11-14 17:18_

Just a question of clarification: the example shown in the description doesn't look like "nested projects" to me? It looks like side-by-side projects? Unless it is implied that there is an outer `ty.toml` or `pyproject.toml` that isn't mentioned.

So is this issue just for supporting "multiple projects open at once in LSP", or is it for supporting nested projects?

---

_Comment by @MichaReiser on 2025-11-14 17:24_

You could also have an outer project which complicates things slightly because you then need to exclude the paths of the inner projects. 

But yes, the main feature is that if you open an outer folder that contains multiple project (nested or side by side), then ty should use the closest ty configuration instead of the closest configuration to the root folder

---

_Comment by @MichaReiser on 2025-11-16 11:54_

I started an implementation in https://github.com/astral-sh/ruff/compare/main...micha/lsp-mono-repo

What's left to do is:

* The current heuristics of creating a db for each `pyproject.toml` seems to eager as it's not clear when there are multiple nested `pyproject.toml` if each member should be checked on their own or if it's a single workspace. I think we want to change the heuristics to ignore `pyproject.toml` files that use the default configuration (after resolving the virtual env path from the editor)
* The server needs to rediscover the projects when the `pythonEnvironment` configuration changes OR when a`pyproject.toml` or `ty.toml` file was created or deleted. 
* We need to call `File::sync` for every open file for every open database if any database was closed (e.g. when a inner project was removed, the files now all belong to the outer project, but it may already have an interned `File` that now needs updating)


One thing that's unclear to me is whether we need to sync the open file state for every database even if the file doesn't belong to this DB instance. For example, a project might import a file from another project (made available through the virtual env). Changes to the imported file (even when unsaved) should be reflected for both databases. 

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:42_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:42_

---

_Assigned to @Gankra by @Gankra on 2026-01-09 03:46_

---

_Comment by @MichaReiser on 2026-01-09 09:13_

I'm not convinced this is the solution as it won't be able to support arbitrary setting differences, but I think it's worth mentioning. 

What I had in mind for uv-workspaces (where all projects use the same search paths) was to heavily lean on overrides to toggle rules (and other settings) at a sub-project level

---

_Comment by @MichaReiser on 2026-01-09 09:22_

Clsoing in favor of 

---

_Closed by @MichaReiser on 2026-01-09 09:22_

---
