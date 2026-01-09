---
number: 16334
title: "Recommend using `--exact` in script shebang lines"
type: issue
state: open
author: treyhunner
labels:
  - needs-decision
assignees: []
created_at: 2025-10-17T03:04:50Z
updated_at: 2025-10-17T08:30:33Z
url: https://github.com/astral-sh/uv/issues/16334
synced_at: 2026-01-07T13:12:19-06:00
---

# Recommend using `--exact` in script shebang lines

---

_Issue opened by @treyhunner on 2025-10-17 03:04_

I found it surprising that removing dependencies from a script with inline metadata does not affect the auto-created virtual environment for that script when the script is run.

After opening #16241 and quite a bit of digging and trial and error, I discovered that `uv sync` supports a `--script` argument and that running sync is *different* from the automatic sync that happens with `uv run`. I now understand that this is by design and than `uv run --exact` can be used to perform exact syncing during a run. None of this is currently mentioned [in the documentation page on running scripts](https://docs.astral.sh/uv/guides/scripts/).

I have been using uv for managing scripts for many months but I am newer to using uv for projects. The default inexact nature of `uv run` makes sense within a project, but I don't think it makes as much sense for a script, since, as a script user I don't even know where the virtual environment for my script is and I didn't even know about `uv sync` until I saw it used in the context of a project.

I propose the documentation for running scripts be updated to:
1. Note `uv run --exact --script <path>` and `uv sync --script <path>`
2. Recommend a shebang line of `#!/usr/bin/env -S uv run --exact --script`

I *really* enjoy uv scripts and how easy they are to use. There are only a few sharp edges I've noticed and this seems like one of them to me.

---

_Comment by @konstin on 2025-10-17 08:30_

I also see the option of making `uv run` exact by default for isolated environments, where it isn't destructive wrt to manually installed extras or packages in the project environment.

---

_Label `needs-decision` added by @konstin on 2025-10-17 08:30_

---

_Referenced in [lalvarezt/uv-helper#1](../../lalvarezt/uv-helper/issues/1.md) on 2025-11-02 21:34_

---
