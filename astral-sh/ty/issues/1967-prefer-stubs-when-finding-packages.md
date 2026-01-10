---
number: 1967
title: "Prefer *-stubs when finding packages"
type: issue
state: open
author: Boon-in-Oz
labels:
  - bug
  - imports
assignees: []
created_at: 2025-12-16T23:42:39Z
updated_at: 2025-12-27T19:31:19Z
url: https://github.com/astral-sh/ty/issues/1967
synced_at: 2026-01-10T01:51:14Z
---

# Prefer *-stubs when finding packages

---

_Issue opened by @Boon-in-Oz on 2025-12-16 23:42_

Hi, this is going to be a hand-wavy issue, sorry.

I'm excited to try ty over Pylance in VS Code. I've immediately run into an issue though.

I run VS Code, and my module, in a Rez environment. I have a wrapped C++ package (a binary called ape.pyd) that I use a lot, which has custom stubs package called ape-stubs. In the Rez environment, PYTHONPATH contains the directories for both the package containing the ape.pyd, and the directory containing ape-stubs, but the pyd directory comes first.

With Pyright/Pylance this used to work. ape-stubs was used for autocompletion etc. However ty appears to be using ape.pyd, and it's not finding any of the useful information in the stubs.

I set the log level to "trace" and saw this in the output (I'm filtering by "ape", and I've condensed the file paths. Also as you can see I have a directory junction from C to S):

```
2025-12-17 10:22:04.872486000 DEBUG ty:main ty_project::metadata::options: Adding `c:\path_to_pyd\packages\ape\1.0\python-3.10` from the `PYTHONPATH` environment variable to `extra_paths`
2025-12-17 10:22:04.872533100 DEBUG ty:main ty_project::metadata::options: Adding `c:\path_to_stubs` from the `PYTHONPATH` environment variable to `extra_paths`
...
2025-12-17 10:22:04.876326600 DEBUG ty:main ty_python_semantic::module_resolver::resolver: Adding extra search-path `S:\path_to_pyd\packages\ape\1.0\python-3.10`
2025-12-17 10:22:04.876435300 DEBUG ty:main ty_python_semantic::module_resolver::resolver: Adding extra search-path `S:\path_to_stubs`
...
2025-12-17 10:22:04.882233300 DEBUG ty:main ruff_db::files::file_root: Adding new file root 'S:\path_to_pyd\packages\ape\1.0\python-3.10' of kind LibrarySearchPath
2025-12-17 10:22:04.882242200 DEBUG ty:main ruff_db::files::file_root: Adding new file root 'S:\path_to_stubs' of kind LibrarySearchPath
...
2025-12-17 10:22:04.885231400 DEBUG ty:main ty_python_semantic::module_resolver::resolver: Adding editable installation to module resolution path S:\path_above_stubs
2025-12-17 10:22:04.885242800 DEBUG ty:main ruff_db::files::file_root: Adding new file root 'S:\path_above_stubs' of kind LibrarySearchPath
```

I _think_ the language server should see that it has a package called `ape` in a search path, and `ape-stubs` in a later search path, and prefer `ape-stubs` even though it found `ape` first.

---

_Comment by @Gankra on 2025-12-16 23:52_

Our code does first search every search-path for a stub, and only then if it fails searches for non-stubs. So whatever's happening here is us getting confused and not finding ape-stubs at all.

---

_Comment by @Gankra on 2025-12-16 23:59_

I'm certainly suspicious that the junction's making us get confused. I'm also suspicious about the fact that `S:\path_to_stubs` and `S:\path_above_stubs` are both things you list in your output. That's a common recipe for us getting confused (https://github.com/astral-sh/ty/issues/1730)

---

_Label `imports` added by @Gankra on 2025-12-17 00:00_

---

_Label `bug` added by @Gankra on 2025-12-17 00:00_

---

_Comment by @Boon-in-Oz on 2025-12-17 01:06_

Thanks for that info. I can attempt some other things to see if I can narrow the junction part down, at least. Regarding that, the recent  change to the VS Code "python.locator" broke the PyLance language server for me and I have to use the setting `"python.locator": "js"` (rather than `native`) for that to work correctly.

Regarding the path I labeled as `path_above_stubs`, I'm not sure where that is coming from. It is the directory above `path_to_stubs` (in case my shorthand is confusing). It might be coming from somewhere else in my environment that I've missed, but it doesn't have the associated line with "from the `PYTHONPATH` environment variable" in the log like the other paths do.

---

_Comment by @Gankra on 2025-12-17 01:12_

The logs suggest that `path_above_stubs` is an editable installed into your venv

---

_Comment by @Boon-in-Oz on 2025-12-17 02:15_

You're correct, thanks. It's in a .pth file. I removed it and while those lines are now gone from the log, the problem of not using the stubs remains.

I have also managed to run my VS Code in a Rez environment build entirely from my S: drive, with no junctions, and I'm still getting the problem. Here's a new log summary:

```
2025-12-17 12:58:17.494912000 DEBUG ty:main ty_project::metadata::options: Adding `s:\path_to_pyd\packages\ape\1.0\python-3.10` from the `PYTHONPATH` environment variable to `extra_paths`
2025-12-17 12:58:17.494946300 DEBUG ty:main ty_project::metadata::options: Adding `S:\path_to_stubs` from the `PYTHONPATH` environment variable to `extra_paths`
...
2025-12-17 12:58:17.497297200 DEBUG ty:main ty_python_semantic::module_resolver::resolver: Adding extra search-path `S:\path_to_pyd\packages\ape\1.0\python-3.10`
2025-12-17 12:58:17.497379900 DEBUG ty:main ty_python_semantic::module_resolver::resolver: Adding extra search-path `S:\path_to_stubs`
...
2025-12-17 12:58:17.500628200 DEBUG ty:main ty_python_semantic::module_resolver::resolver: Using vendored stdlib
2025-12-17 12:58:17.500930300 DEBUG ty:main ty_python_semantic::module_resolver::resolver: Adding site-packages search path `S:\...\python3.10.11\Lib\site-packages`
2025-12-17 12:58:17.500994500  INFO ty:main ty_project::metadata::options: Python version: Python 3.10, platform: win32
...
2025-12-17 12:58:17.501120000 DEBUG ty:main ruff_db::files::file_root: Adding new file root 'S:\path_to_pyd\packages\ape\1.0\python-3.10' of kind LibrarySearchPath
2025-12-17 12:58:17.501128900 DEBUG ty:main ruff_db::files::file_root: Adding new file root 'S:\path_to_stubs' of kind LibrarySearchPath
...
2025-12-17 12:58:17.501762600 DEBUG ty:main ty_server::session: Registering diagnostic capability with OpenFilesOnly diagnostic mode
2025-12-17 12:58:17.501810100 DEBUG ty:main ty_python_semantic::module_resolver::resolver: Resolving dynamic module resolution paths
2025-12-17 12:58:17.502208600 DEBUG ty:main ty_python_semantic::module_resolver::resolver: Adding editable installation to module resolution path ...
...
2025-12-17 12:58:17.506657700 DEBUG ty:main ty_server::server::main_loop: Processing deferred notification `textDocument/didOpen`
2025-12-17 12:58:17.506685500  INFO ty:main notification{method="textDocument/didOpen"}: ty_server::session: Initializing the default project
2025-12-17 12:58:17.507848300  INFO ty:main notification{method="textDocument/didOpen"}: ty_project::metadata::options: Defaulting to python-platform `win32`
...
2025-12-17 12:58:17.508598800 DEBUG ty:main notification{method="textDocument/didOpen"}: ty_project::metadata::options: Adding `s:\path_to_pyd\packages\ape\1.0\python-3.10` from the `PYTHONPATH` environment variable to `extra_paths`
2025-12-17 12:58:17.508630500 DEBUG ty:main notification{method="textDocument/didOpen"}: ty_project::metadata::options: Adding `S:\path_to_stubs` from the `PYTHONPATH` environment variable to `extra_paths`
...
2025-12-17 12:58:17.510881800 DEBUG ty:main notification{method="textDocument/didOpen"}: ty_python_semantic::module_resolver::resolver: Adding extra search-path `S:\path_to_pyd\packages\ape\1.0\python-3.10`
2025-12-17 12:58:17.510964100 DEBUG ty:main notification{method="textDocument/didOpen"}: ty_python_semantic::module_resolver::resolver: Adding extra search-path `S:\path_to_stubs`
...
2025-12-17 12:58:17.514290600 DEBUG ty:main notification{method="textDocument/didOpen"}: ty_python_semantic::module_resolver::resolver: Using vendored stdlib
2025-12-17 12:58:17.514540400  INFO ty:main notification{method="textDocument/didOpen"}: ty_project::metadata::options: Python version: Python 3.14, platform: win32
...
2025-12-17 12:58:17.514664700 DEBUG ty:main notification{method="textDocument/didOpen"}: ruff_db::files::file_root: Adding new file root 'S:\path_to_pyd\packages\ape\1.0\python-3.10' of kind LibrarySearchPath
2025-12-17 12:58:17.514673400 DEBUG ty:main notification{method="textDocument/didOpen"}: ruff_db::files::file_root: Adding new file root 'S:\path_to_stubs' of kind LibrarySearchPath
```
I've attached a full log, similarly redacted.

[vscode_ty_log_launch_from_s.txt](https://github.com/user-attachments/files/24203119/vscode_ty_log_launch_from_s.txt)

---

_Comment by @Boon-in-Oz on 2025-12-17 02:25_

As an experiment, I copied the `ape-stubs` directory from `S:\path_to_stubs` to `s:\path_to_pyd\packages\ape\1.0\python-3.10`, and ty now reads the stubs correctly. I'm not sure if that will be a possible long-term fix for us, but hopefully it tells you more about the problem.

---

_Comment by @MichaReiser on 2025-12-18 10:09_

I think the issue here is simply the ordering of the paths. Could you try changing `PYTHONPATH` so that the stubs path comes before `S:\path_to_pyd\packages\ape\1.0\python-3.10` (which should make ty try the stubs path first)

---

_Comment by @Boon-in-Oz on 2025-12-18 20:31_

I don't think that's an option for me, because my PYTHONPATH is built by Rez.

I think the language server should see that it has a package called ape in a search path, and ape-stubs in a later search path, and prefer ape-stubs even though it found ape first. That appears to be how PyRight does it, anyhow.

---

_Comment by @carljm on 2025-12-24 00:09_

I think @Boon-in-Oz has the right analysis here. According to https://typing.python.org/en/latest/spec/distributing.html#import-resolution-ordering a `foo-stubs` package should always take priority over a `foo` package, regardless of search path ordering. It looks like we implemented this wrong: we are going search-path-entry by search-path-entry looking first for `foo-stubs` and then for `foo`, where instead we need to hoist the `-stubs` handling out to the top level: first try an entire resolve (on all search paths) for `foo-stubs`, and then for `foo`.

Here is a playground that should type-check cleanly but currently doesn't: https://play.ty.dev/64f64de3-bd08-4007-ac1f-7899d91b5c48

---

_Added to milestone `Stable` by @carljm on 2025-12-24 00:09_

---

_Comment by @MichaReiser on 2025-12-24 07:23_

> According to [typing.python.org/en/latest/spec/distributing.html#import-resolution-ordering](https://typing.python.org/en/latest/spec/distributing.html#import-resolution-ordering) a foo-stubs package should always take priority over a foo package, regardless of search path ordering.

If I understand the spec correctly that's only the case for third-party search paths (and they should be ignored for first-party or extra-paths?). 





---

_Comment by @carljm on 2025-12-27 19:31_

> If I understand the spec correctly that's only the case for third-party search paths (and they should be ignored for first-party or extra-paths?).

Yeah, it looks like that is the intent of the spec. Would be interesting to see what other type checkers actually do here.

---
