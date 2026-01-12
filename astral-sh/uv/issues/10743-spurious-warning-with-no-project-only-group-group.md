```yaml
number: 10743
title: "Spurious warning with `--no-project --only-group <group>`?"
type: issue
state: open
author: woodruffw
labels:
  - question
assignees: []
created_at: 2025-01-18T20:24:13Z
updated_at: 2025-01-20T03:44:57Z
url: https://github.com/astral-sh/uv/issues/10743
synced_at: 2026-01-12T16:00:20Z
```

# Spurious warning with `--no-project --only-group <group>`?

---

_@woodruffw_

Hello! As always, thanks for all the amazing work you've done on `uv` and `ruff`.

I tried searching for an open issue for this, and couldn't find one. Please close with prejudice if there is one that I've missed!

## Problem

This isn't a breaking problem per se, since it appears to work. However, it produces a spurious warning that I suspect is a holdover from an earlier restriction 🙂 

I have a `pyproject.toml` that looks like this:

```toml
[build-system]
requires = ["maturin>=1.0,<2.0"]
build-backend = "maturin"

[tool.maturin]
bindings = "bin"

[dependency-groups]
docs = ["mkdocs ~= 1.6", "mkdocs-material[imaging] ~= 9.5"]
```

Notably, the `pyproject.toml` has no `[project]` section (since it's a Maturin-based pure-Rust project that pulls all metadata from `Cargo.toml`). My goal is to ideally keep it that way, to reduce duplication between the two files.

With that configuration, I'm using `uv run --no-project --only-group docs mkdocs build` to build my (`mkdocs`-based) documentation.

## Expected behavior

I expect `uv run --no-project --only-group docs mkdocs build` to produce no warnings, and run `mkdocs` correctly.

## Actual behavior

The command warnings that `--only-group docs` and `--no-project` have no effect alongside each other, but then runs successfully:

```bash
$ uv run --no-project --only-group docs mkdocs build
warning: `--only-group docs` has no effect when used alongside `--no-project`
INFO    -  Cleaning site directory
INFO    -  Building documentation to directory: /<blah>/zizmor/site_html
INFO    -  Documentation built in 0.84 seconds
```

## Workarounds

I can add a dummy `[project]` section containing just a `name` key, which hushes the warning. However I think the warning is still spurious, since (IIUC) the semantics of PEP 735 dep groups are not dependent on project metadata at all.


---

_Comment by @woodruffw on 2025-01-18 20:26_

Actually, it looks like caching may have been masking a larger problem here: when I run the same `--no-project --only-group docs` command in CI, I get a command error for `mkdocs`:

```
uv run --no-project --only-group docs mkdocs build
warning: `--only-group docs` has no effect when used alongside `--no-project`
error: Failed to spawn: `mkdocs`
  Caused by: No such file or directory (os error 2)
make: *** [Makefile:7: site] Error 2
Error: Process completed with exit code 2.
```

(CI logs: https://github.com/woodruffw/zizmor/actions/runs/12847160565/job/35823153622)

Whereas locally this command succeeds.

---

_Comment by @charliermarsh on 2025-01-18 23:19_

Hmm, I think this is intentional behavior -- I wouldn't expect us to discover that `pyproject.toml` if it's lacking a `[project]` or `[tool.uv.workspace]` table. I'm guessing that if you run with `--verbose`, it says something like `No project found`? We don't have first-class support for "projects without a `[project]` table right now.

---

_Label `question` added by @charliermarsh on 2025-01-18 23:19_

---

_Comment by @woodruffw on 2025-01-19 00:14_

Aha, indeed!

```
WARN Ignoring project discovery error due to `--no-project`: No `project` table found in: `/home/william/devel/zizmor/pyproject.toml`
warning: `--only-group docs` has no effect when used alongside `--no-project`
```

In that case, I _think_ this is a feature request: it'd be great to be able to express `dependency-groups` in a `pyproject.toml` without requiring a `[project]` block!

But I can also understand if this is not a well-defined part of packaging, and I have a perfectly functional workaround 🙂 

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-20 03:44_

---
