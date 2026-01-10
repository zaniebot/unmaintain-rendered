---
number: 17001
title: "Add helpful error message for 'uv activate'"
type: pull_request
state: open
author: nooscraft
labels: []
assignees: []
base: main
head: feature/uv-activate-helpful-error
created_at: 2025-12-05T16:05:03Z
updated_at: 2025-12-07T06:30:54Z
url: https://github.com/astral-sh/uv/pull/17001
synced_at: 2026-01-10T01:26:19Z
---

# Add helpful error message for 'uv activate'

---

_Pull request opened by @nooscraft on 2025-12-05 16:05_

When users try to run 'uv activate', show a helpful error message
instead of the generic unknown command error. The message explains
that 'uv activate' is not supported and provides instructions on
how to activate a virtual environment.

This is most probably address the issue: https://github.com/astral-sh/uv/issues/16993

---

_Comment by @nooscraft on 2025-12-05 16:05_

@charliermarsh @zanieb FYI!

---

_Comment by @nooscraft on 2025-12-05 16:09_


```
   error `uv activate` is not supported. To activate a virtual environment, use:
     source .venv/bin/activate

   hint Create a virtual environment with: uv venv
```

---

_Review comment by @darth-raijin on `crates/uv/src/lib.rs`:2493 on 2025-12-05 17:06_

Nit: The code already makes this intent pretty clear, so these two comments might be redundant. Fine to keep, but you could consider removing.

---

_@darth-raijin reviewed on 2025-12-05 17:07_

Small suggestion about some possibly redundant comments

---

_Comment by @zanieb on 2025-12-05 17:27_

I'm still not sure we should do this, but if we do, we probably need to actually use our shell detection logic as well as discover the virtual environment to ensure one exists?

---

_Comment by @darth-raijin on 2025-12-05 17:32_

+1 on the above. 

It feels like it could set a precedent for adding similar custom messages for other unsupported commands, instead of handling them in a more general way.

---

_Comment by @nooscraft on 2025-12-07 06:23_

Hey, updated this based on @zanieb's feedback!

Now it actually detects your shell and finds the venv instead of just printing static text. Walks up from cwd looking for `.venv/pyvenv.cfg`, checks `ZSH_VERSION`/`FISH_VERSION`/etc to figure out what shell you're in, and gives you the right command.

Quick examples:

```
$ uv activate
error `uv activate` is not supported. To activate a virtual environment, use:
  source .venv/bin/activate
```

If you're in fish:
```
$ uv activate
error `uv activate` is not supported. To activate a virtual environment, use:
  source .venv/bin/activate.fish
```

No venv? Shows a hint:
```
$ uv activate
error `uv activate` is not supported. To activate a virtual environment, use:
  source .venv/bin/activate

hint Create a virtual environment with: uv venv
```

@darth-raijin - fair point on precedent, but we already do this for `install`, `show`, `freeze` etc (they suggest `uv pip <cmd>`). This one's a bit different since there's no actual command to suggest - just need to explain why `uv activate` can't be a thing and point them to the shell command.

---
