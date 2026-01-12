```yaml
number: 1281
title: "Use `--stdin-filename` when resolving configuration files"
type: pull_request
state: merged
author: cdbrendel
labels: []
assignees: []
merged: true
base: main
head: stdin-filename-config
created_at: 2022-12-18T20:09:33Z
updated_at: 2022-12-18T22:54:28Z
url: https://github.com/astral-sh/ruff/pull/1281
synced_at: 2026-01-12T15:55:06Z
```

# Use `--stdin-filename` when resolving configuration files

---

_@cdbrendel_

Currently, when `ruff` is called with  `--stdin-filename`, it does not use this value when searching for a configuration file:

```console
user@host ~/ $ tree
.
├── foo
│   ├── bar.py
│   └── pyproject.toml
└── pyproject.toml
```

```console
user@host ~/ $ cat /home/user/foo/bar.py | ruff --stdin-filename=/home/user/foo/bar.py -
...
# Uses /home/user/pyproject.toml (based on cwd /home/user), not the expected /home/user/foo/pyproject.toml
```

This is a non-trivial issue when using `ruff` with, for example, something like [null-ls.nvim](https://github.com/Carlosiano/null-ls.nvim), which passes files via stdin and correctly sets `--stdin-filename`, but does not set the current directory to the project directory before executing `ruff`. 

By contrast, when `black` receives a `--stdin-filename` flag, it uses it in the resolution of the project root (see [here](https://github.com/psf/black/blob/cd9fef8bab3a39dfba494f1917e4158faba439f9/src/black/files.py#L43-L61)). So, `black` finds the expected project-local `pyproject.toml` in the example directory tree above:

```console
user@host ~/ $  cat /home/user/foo/bar.py | black --stdin-filename=/home/user/foo/bar.py -v -
Identified `/home/user/foo` as project root containing a pyproject.toml. 
...
```

This PR makes `ruff` find configuration files more like `black` does when `--stdin-filename` is passed. Apologies in advance if the current behavior of `ruff` is intended or if this PR is premature (and I won't be offended if this PR is rejected!). 


---

_Renamed from "Use `--stdin-filename` for resolving configuration files" to "Use `--stdin-filename` when resolving configuration files" by @cdbrendel on 2022-12-18 20:09_

---

_Marked ready for review by @cdbrendel on 2022-12-18 20:11_

---

_Comment by @charliermarsh on 2022-12-18 22:47_

Total oversight, thank you for the fix!

---

_Comment by @charliermarsh on 2022-12-18 22:47_

Will get this out tonight.

---

_Comment by @cdbrendel on 2022-12-18 22:51_

Great! Thanks for all your hard work  @charliermarsh :)

---

_Merged by @charliermarsh on 2022-12-18 22:51_

---

_Closed by @charliermarsh on 2022-12-18 22:51_

---

_Comment by @charliermarsh on 2022-12-18 22:52_

It's my pleasure! Hope you're enjoying Ruff :)

---

_Branch deleted on 2022-12-18 22:54_

---
