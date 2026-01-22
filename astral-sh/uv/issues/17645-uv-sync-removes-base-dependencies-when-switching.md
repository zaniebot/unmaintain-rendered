```yaml
number: 17645
title: "`uv sync` removes base dependencies when switching extras (at least impacts typer/typer-slim)"
type: issue
state: open
author: pimlock
labels:
  - bug
assignees: []
created_at: 2026-01-21T19:40:22Z
updated_at: 2026-01-21T23:09:56Z
url: https://github.com/astral-sh/uv/issues/17645
synced_at: 2026-01-22T00:09:02Z
```

# `uv sync` removes base dependencies when switching extras (at least impacts typer/typer-slim)

---

_@pimlock_

### Summary

# Summary

When switching extras with `uv sync` (or `uv tool install`), packages that provide conflicting files (like `typer` and `typer-slim`) can corrupt the venv. Installing `typer-slim` overwrites `typer`'s files, and uninstalling `typer-slim` deletes them—leaving uv's metadata incorrectly showing typer as installed while the actual files are gone.

# Reproduction

I created a repo with the repro example: https://github.com/pimlock/uv-bug-repro

When this happened in my workflow, the dependency tree was more complex, the base package depended on `typer` and one of deps defined in extra had `typer-slim` as a dep few levels deep.

Here's the verbose output when running all the commands with the spot where the "issue" happens being highlighted
  - https://github.com/pimlock/uv-bug-repro/blob/main/repro-output.txt#L180-L192

Commands:
```
git clone https://github.com/pimlock/uv-bug-repro
cd uv-bug-repro

uv sync                     # ✓ works
uv sync --extra extra1      # ✓ works
uv sync --extra extra2      # ✗ ModuleNotFoundError: No module named 'typer'
```

The package has:
- Base dependency: typer>=0.20.0
- extra1: typer-slim>=0.20.0
- extra2: rich>=13.0.0 (note: what deps extra2 has is irrelevant for this issue)

## What happens

1. `uv sync` installs typer (which **doesn't** depend on `typer-slim`)
2. `uv sync --extra extra1` adds `typer-slim`
3. `uv sync --extra extra2` removes `typer-slim`, but because they "contribute" the same module in venv, the `typer`'s files get removed as well.

This issue is likely caused by how `typer` and `typer-slim` are set up (i.e. `typer` doesn't depend on `typer-slim`), but it may be potentially causing similar issues with other packages (AFAIK pydantic uses the `-slim` pattern, but in a different way).

- typer requires: click, typing-extensions, shellingham, rich
- typer-slim requires: click, typing-extensions

They conflict because they both install files to the same typer/ directory. So here's what's happening:

1. Step 1: `typer` installed → typer/ directory created
2. Step 2: `typer-slim` installed → overwrites typer/ directory
3. Step 3: `typer-slim` uninstalled → deletes typer/ directory

# Expected

Base dependency typer>=0.20.0 should always be installed regardless of which extras are selected.

# Workaround

`uv sync --extra extra2 --reinstall` works correctly.



### Platform

Darwin 25.2.0 arm64

### Version

uv 0.9.26 (ee4f003 2026-01-15)

### Python version

CPython 3.14.0

---

_Label `bug` added by @pimlock on 2026-01-21 19:40_

---

_Comment by @zanieb on 2026-01-21 20:38_

Thanks for the report and reproduction!

> Installing typer-slim overwrites typer's files, and uninstalling typer-slim deletes them—leaving uv's metadata incorrectly showing typer as installed while the actual files are gone.

It's hard to say if we should support this — we generally discourage that pattern as it's problematic in a variety of ways.

cc @konstin who has more context on this category of issue.


---

_Comment by @zanieb on 2026-01-21 20:48_

Here the issue is not that we're removing base dependencies, but rather that we're not reinstalling `typer` after uninstalling `typer-slim` which overwrites typer's files?

---

_Comment by @pimlock on 2026-01-21 21:10_

> Here the issue is not that we're removing base dependencies, but rather that we're not reinstalling typer after uninstalling typer-slim which overwrites typer's files?

Yes, that's right. I initially thought that `typer` depended on `typer-slim` and that the issue was related to how that relationship was handled, but then I realized that wasn't the case, these 2 are completely unrelated in terms of deps.

Running with `--verbose` ([here](https://github.com/pimlock/uv-bug-repro/blob/main/repro-output.txt#L190-L192)) was when I realized the issue is that both packages use the same module name `typer` (`venv/lib/python/site-packages/typer`), so by uninstalling `typer-slim` that dir gets deleted.

---

_Comment by @pimlock on 2026-01-21 22:54_

One more bit of context: I tried the same flow with pip and that worked, i.e. left the venv in a working state.

```shell
# in that sample repo
source .venv/bin/activate
pip install "."
pip install ".[extra1]"
pip install ".[extra2]"
repro-cli # works!
```

Interestingly, same is true when running through `uv pip ...`

```shell
source .venv/bin/activate
uv pip install "."
uv pip install ".[extra1]"
uv pip install ".[extra2]"
repro-cli # works!
```

---

_Comment by @zanieb on 2026-01-21 23:03_

`pip install` and `uv pip install` both won't remove any extraneous files, so `typer-slim` is just not being removed, I presume?

---

_Comment by @pimlock on 2026-01-21 23:08_

Yes, that seems to be the case:
```
uv pip install ".[extra2]"
Resolved 9 packages in 4ms
      Built uv-bug-repro @ file:///Users/pmlocek/dev/tries/2026-01-21-uv-repro
Prepared 1 package in 435ms
Uninstalled 1 package in 2ms
Installed 1 package in 6ms
 ~ uv-bug-repro==0.1.0 (from file:///Users/pmlocek/dev/tries/2026-01-21-uv-repro)
```

vs

```
uv sync --extra extra2
Resolved 11 packages in 2ms
Uninstalled 1 package in 6ms
 - typer-slim==0.21.1
```

Also, that reminded me of `--inexact`, which also doesn't remove extraneous stuff, so `uv sync --extra extra2 --inexact` leaves `typer` module in venv.

---

_Comment by @zanieb on 2026-01-21 23:09_

Yeah the problem with that as the "fix" is that if `typer-slim` mutates any files from `typer` then the mutated file will remain and could break the package.

One fix is that we can identify that satisfied candidates need to be "reinstalled" if an uninstalled candidate has an overlapping file, here's a poc https://github.com/astral-sh/uv/compare/main...zaniebot:claude/reproduce-uv-17645-7DkMp

---
