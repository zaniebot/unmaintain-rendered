---
number: 9817
title: "Skip `python.exe` with `uv install python --default`"
type: issue
state: open
author: Vynce
labels:
  - configuration
assignees: []
created_at: 2024-12-11T17:19:19Z
updated_at: 2024-12-11T21:52:03Z
url: https://github.com/astral-sh/uv/issues/9817
synced_at: 2026-01-07T13:12:18-06:00
---

# Skip `python.exe` with `uv install python --default`

---

_Issue opened by @Vynce on 2024-12-11 17:19_

I love the `uv python install --default` feature (https://github.com/astral-sh/uv/pull/8650), but it creates a `python.exe` executable in addition to a `python3.exe` executable. Unfortunately, we still need `python.exe` to resolve to a Python 2.7 interpreter for the time being. Would you be willing to add a `uv.toml` setting or an alternative `--default-py3only` arg or something that limits the feature to only managing `python3.exe`. Similar issue on Linux, but I'm specifically looking at the Windows behavior.

---

_Comment by @zanieb on 2024-12-11 17:50_

Oof. Hm.. I guess a `uv.toml` setting is less controversial to me (it seems niche for the CLI), but we don't have Python install settings there yet.

Would an environment variable be sufficient?

> Unfortunately, we still need python.exe to resolve to a Python 2.7 interpreter for the time being. 

Would it be feasible to specify the full path to your 2.7 interpreter? Or adjust the `PATH` in that context?

Thanks for reporting!

---

_Label `configuration` added by @zanieb on 2024-12-11 17:50_

---

_Comment by @Vynce on 2024-12-11 21:20_

> Would an environment variable be sufficient?

Yeah, that could work. We could set that globally in our uv installation script (not that different from setting it in the `uv.toml`). We're already setting the default index url and python-install-mirror in the `uv.toml`.

> Would it be feasible to specify the full path to your 2.7 interpreter? Or adjust the `PATH` in that context?

Not easily. We need to support a mixed environment where legacy scripts expect `python` to be Python 2.7. I really want `~/.local/bin` to be first in the `PATH` since that's where all the uv goodies live, but a `python.exe` there will shadow others later in the `PATH`. Putting Python 2.7 before `~/.local/bin` on the `PATH` could work, but introduces other complexities with managing `PATH` order. Dealing with all this nonsense with Python on Windows is why I'm so excited about uv 😄.

Also feel free to decide that uv isn't going to go out of its way to support coexistence with Python 2. That'll make me sad because `--default` is exactly the feature I want to use to manage Python 3 versions going forward, but I can create my own `python3.exe` symlink that points to the desired uv trampoline executable instead or come up with other workarounds 😊.

---

_Comment by @zanieb on 2024-12-11 21:26_

Another question, is there a way you're invoking `python.exe` for Python 2 that would inform us that we should pass that invocation through to another executable?

I think some flag here is reasonable, just need to figure out how to express it.

---

_Comment by @Vynce on 2024-12-11 21:52_

Python 2 is typically invoked via the shebang in the the Python script (e.g. `#!/usr/bin/env python`) in a Git Bash (aka msys bash) shell on Windows. We do it this way so it works the same on Linux and macOS. There are some shell scripts that explicitly call Python 2 scripts directly, like `python myscript.py`. Both cases assume that `python` is on the `PATH` and resolves to Python 2.

---
