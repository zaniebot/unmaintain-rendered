---
number: 2556
title: "`INP001` for top level `setup.py`, `manage.py`, `noxfile.py`"
type: issue
state: closed
author: scop
labels: []
assignees: []
created_at: 2023-02-03T21:40:42Z
updated_at: 2023-04-10T09:17:00Z
url: https://github.com/astral-sh/ruff/issues/2556
synced_at: 2026-01-07T13:12:14-06:00
---

# `INP001` for top level `setup.py`, `manage.py`, `noxfile.py`

---

_Issue opened by @scop on 2023-02-03 21:40_

ruff 0.0.240 issues `INP001` for some commonly found top level files, such as

- `setup.py` ("legacy" setuptools etc)
- `manage.py` (Django)
- `noxfile.py` (Nox)

I don't think these warrant adding a top level `__init__.py`, and suggest ignoring them as far as `INP001` is concerned. (In fact, I'm not sure if this should be actually done for the top level dir in any case no matter what's in it, but I admit I haven't thought much about it at all and can't tell offhand.)

This would be a deviation from upstream behavior, but a sensible one in my opinion. Upstream documentation contains a [recipe mentioning `manage.py`](https://github.com/adamchainz/flake8-no-pep420/#inp001-file-is-part-of-an-implicit-namespace-package-add-__init__py).

---

_Comment by @edgarrmondragon on 2023-02-03 22:00_

A heuristic may be to ignore those names at the root level (not sure how to detect that, though)

---

_Comment by @charliermarsh on 2023-02-03 22:11_

We could in theory use `src` for this...? See if anything is directly in a `src` directory, and ignore it?

---

_Comment by @edgarrmondragon on 2023-02-03 22:21_

> We could in theory use `src` for this...? See if anything is directly in a `src` directory, and ignore it?

`noxfile.py` and `setup.py` could be outside of the `src` directory, though. Not sure about `manage.py`.

---

_Comment by @scop on 2023-02-03 22:33_

Right, those two are actually very likely always in the top level dir, no matter what `src` is set to. I believe the same applies to `manage.py`.

Perhaps ignore everything in the top level dir _and_ all directly in a `src` one, irrespective of the file names?

---

_Comment by @charliermarsh on 2023-02-03 23:28_

What's the different between the top-level dir and the `src` directories? (Here, I'm referring to `src = ["."]`, not a literal `./src` directory, just in case that was unclear.)

---

_Comment by @edgarrmondragon on 2023-02-03 23:46_

> What's the different between the top-level dir and the `src` directories? (Here, I'm referring to `src = ["."]`, not a literal `./src` directory, just in case that was unclear.)

Ah, now that makes more sense 😅. Yeah, it's probably safe to ignore those files directly under the `src` directories.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-04 00:10_

---

_Referenced in [astral-sh/ruff#2560](../../astral-sh/ruff/pulls/2560.md) on 2023-02-04 00:18_

---

_Closed by @charliermarsh on 2023-02-04 00:20_

---

_Comment by @scop on 2023-02-04 11:31_

By "top level" dir I mean the project's root dir, no matter what `src` is set to.

So if we have a project using the common "src" layout, we want to configure ruff with `src = ["src"]`. But at least `setup.py` and `noxfile.py` are pretty much always in the "top level" dir in such projects, not in `./src/`, and ruff after #2560 still issues `INP001` for them.

So to reiterate, I think all files directly in *both* `./` (== "top level", or "project root dir") and whatever is included in the `src` config, should be ignored. The latter is taken care of by #2560, the former is currently unaddressed.

Example affected project: https://github.com/scop/pytekukko, with the related `noqa` at https://github.com/scop/pytekukko/blob/ccd7c06ee121e5b70fa455ec5e718d4f27707664/noxfile.py#L1 removed:

```shellsession
$ cargo run .../pytekukko
    Finished dev [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/ruff .../pytekukko`
warning: Detected debug build without --no-cache.
.../pytekukko/noxfile.py:1:1: INP001 File `.../pytekukko/noxfile.py` is part of an implicit namespace package. Add an `__init__.py`.
```

---

_Referenced in [astral-sh/ruff#2565](../../astral-sh/ruff/pulls/2565.md) on 2023-02-04 13:21_

---

_Comment by @charliermarsh on 2023-02-04 13:21_

Fixed in #2565.

---

_Comment by @scop on 2023-02-05 12:35_

Nice, thanks!

---

_Comment by @sshishov on 2023-04-09 07:34_

`manage.py` usually located not in the `project_root` but in the `src root` as project root has CI configuration and all other related stuff, and inside SRC we have all the code related to running the service (`manage.py` is kind of an entry point for django)

---

_Comment by @scop on 2023-04-10 04:07_

I can see how a repo might be set up that way, but in my experience the setups having `manage.py` in the project top level dir vastly outnumber ones having it elsewhere.

---

_Comment by @sshishov on 2023-04-10 09:17_

@scop and @charliermarsh no worry guys. This issue can be easily fixed by applying per-file ignore in the `pyproject.toml`

Therefore please just ignore my comment.

P.S.: To preserve original `manage.py` file from `django-admin startproject` we have to apply multiple exceptions anyways...
Have to double check if in new version of Django the format has been changed though 😕 
```
[tool.ruff.per-file-ignores]
"manage.py" = ["I002", "INP001", "ISC003"]
```

---
