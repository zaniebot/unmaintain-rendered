---
number: 3781
title: "Inconsistent results running ruff via `poetry` and `pre-commit`"
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2023-03-28T18:33:38Z
updated_at: 2023-03-28T19:56:33Z
url: https://github.com/astral-sh/ruff/issues/3781
synced_at: 2026-01-07T13:12:14-06:00
---

# Inconsistent results running ruff via `poetry` and `pre-commit`

---

_Issue opened by @ghost on 2023-03-28 18:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I have ruff set up in my (monorepo's) `.pre-commit-config.yaml`, and this runs and I see a bunch of problems! Yay? For instance:

``` shell
% pre-commit run --files app/tools.py
# bunch of junk
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

projects/role_explorer/app/tools.py:65:21: PLR2004 Magic value used in comparison, consider replacing 365 with a constant variable
projects/role_explorer/app/tools.py:71:34: ANN401 Dynamically typed expressions (typing.Any) are disallowed in `v`
```
Sweet! But! I'm also trying to make sure I'm not too startled by large problems that ruff finds, so I try this:
``` shell
% poetry run ruff check app/tools.py ; and echo "wat" # I use fish
wat
```
huh? I would expect `ruff` in both cases to act identically.

If I dump the config with `--show-settings` I see all my changes from my `pyproject.toml`, but they're seemingly ignored when I run ruff? I'm really confused here.

Thanks? Thoughts?

---

_Comment by @ghost on 2023-03-28 18:44_

This is version 0.0.259, the same in both places.

---

_Comment by @ghost on 2023-03-28 18:45_

I turned on `select = ["ALL"]` for the purposes of this demonstration. 

---

_Comment by @ghost on 2023-03-28 19:28_

OK, more oddness. The project setup looks like:

```
monorepo/.pre-commit-config.yaml
monorepo/projects/project_name
monorepo/projects/project_name/pyproject.toml
monorepo/projects/project_name/app/tools.py
```
If I run `ruff check app/tools.py` from the `project_name` directory, nothing happens (the errors in `tools.py` are not caught). If I run `ruff check projects/project_name/app/tools.py`, everything works as expected. Even specifying the configuration (`ruff check --config ./pyproject.toml app/tools.py`) does nothing. I can see that the `toml` is being read in both cases, because when I dump the config with `--show-settings`, I get identical output *regardless* of where I'm running ruff. This is very odd!


---

_Comment by @ghost on 2023-03-28 19:31_

This is on a Mac, btw:
``` shell
% uname -a
Darwin moominmama.local 22.4.0 Darwin Kernel Version 22.4.0: Mon Mar  6 20:59:28 PST 2023; root:xnu-8796.101.5~3/RELEASE_ARM64_T6000 arm64
```

---

_Comment by @charliermarsh on 2023-03-28 19:41_

Is this in a public repo? Not obvious to me what’s going on but happy to debug.

---

_Comment by @ghost on 2023-03-28 19:42_

gaaaaaaaaaaaah please ignore I am very dumb (`fix-only` is a thing!)

---

_Closed by @ghost on 2023-03-28 19:42_

---

_Comment by @charliermarsh on 2023-03-28 19:56_

Aw man, you probably fell victim to one of the edge cases in configuration, which is that there are a few `pyproject.toml` fields that are only respected when you run from the current directory (`fix-only`, fix`, maybe one or two others). All other fields work as expected regardless of where you invoke Ruff from.

---
