```yaml
number: 11009
title: "Improve SIGINT handling in `uv run`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/signals
created_at: 2025-01-28T00:36:30Z
updated_at: 2025-01-28T20:04:15Z
url: https://github.com/astral-sh/uv/pull/11009
synced_at: 2026-01-10T11:45:23Z
```

# Improve SIGINT handling in `uv run`

---

_Pull request opened by @zanieb on 2025-01-28 00:36_

There should be two functional changes here:

- If we receive SIGINT twice, forward it to the child process
- If the `uv run` child process changes its PGID, then forward SIGINT

Previously, we never forwarded SIGINT to a child process. Instead, we
relied on shell to do so.

On Windows, we still do nothing but eat the Ctrl-C events we receive.
I cannot see an easy way to send them to the child.

The motivation for these changes should be explained in the comments.

Closes https://github.com/astral-sh/uv/issues/10952 (in which Ray changes its PGID)
Replaces the (much simpler) #10989 with a more comprehensive approach.

See https://github.com/astral-sh/uv/pull/6738#issuecomment-2315451358 for some previous context.

---

_Comment by @zanieb on 2025-01-28 00:40_

Frankly, I'm still not entirely sure why we wouldn't forward _all_ signals in the same way that we do with SIGTERM. If it wasn't a huge pain, I would probably do it today? But since it is I might just wait until someone complains that something isn't working?

---

_Comment by @zanieb on 2025-01-28 16:05_

A downside of forwarding Ctrl-C on the second send is that a process can receive it _three_ times... but I don't know of programs that have special-casing for that.

---

_Comment by @zanieb on 2025-01-28 17:00_

The first SIGINT is not forwarded (but is received because the shell sends it directly). The second SIGINT is forwarded

```python
import signal, os; 

print(f'PID: {os.getpid()}');
print(f'GPID: {os.getpgid(os.getpid())}');

# We display a newline before SIGINT to separate from ^C in shells
signal.signal(signal.SIGINT, lambda s, _: print(f'{signal.Signals(s).name}', flush=True))
signal.signal(signal.SIGTERM, lambda s, _: print(f'{signal.Signals(s).name}', flush=True))

signal.valid_signals

# Hang until we receive an input, e.g., Ctrl-D
try:
    input()
except EOFError:
    pass

```

```
❯ uv run -v example.py
DEBUG uv 0.5.24+8 (adf534009 2025-01-27)
DEBUG Found project root: `/Users/zb/workspace/uv`
DEBUG Project `uv` is marked as unmanaged
DEBUG No project found; searching for Python interpreter
DEBUG Reading Python requests from version file at `/Users/zb/workspace/uv/.python-version`
DEBUG Searching for Python 3.12 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.8-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.8 interpreter at: /Users/zb/workspace/uv/.venv/bin/python3
DEBUG Running `python example.py`
PID: 84848
GPID: 84847
^CSIGINT
DEBUG Received SIGINT, assuming the child received it as part of the process group
^CSIGINT
DEBUG Received SIGINT, forwarding to child at 84848
SIGINT
```

SIGTERM is forwarded immediately (and only received once)

```
❯ uv run -v example.py
DEBUG uv 0.5.24+8 (adf534009 2025-01-27)
DEBUG Found project root: `/Users/zb/workspace/uv`
DEBUG Project `uv` is marked as unmanaged
DEBUG No project found; searching for Python interpreter
DEBUG Reading Python requests from version file at `/Users/zb/workspace/uv/.python-version`
DEBUG Searching for Python 3.12 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.8-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.8 interpreter at: /Users/zb/workspace/uv/.venv/bin/python3
DEBUG Running `python example.py`
PID: 85335
GPID: 85334
DEBUG Received SIGTERM, forwarding to child at 85335
SIGTERM
```

When the PGID of the child is changed, we forward the first SIGINT 

```python
import signal, os; 

print(f'PID: {os.getpid()}');

# Change PGID
os.setpgid(0, 0)

print(f'GPID: {os.getpgid(os.getpid())}');

signal.signal(signal.SIGINT, lambda s, _: print(f'{signal.Signals(s).name}', flush=True))
signal.signal(signal.SIGTERM, lambda s, _: print(f'{signal.Signals(s).name}', flush=True))

# Sleep for 20s; we can't use `input()` or it breaks output for some reason
try:
    import time
    time.sleep(20)
except EOFError:
    pass
```

```
❯ uv run -v example.py
DEBUG uv 0.5.24+8 (adf534009 2025-01-27)
DEBUG Found project root: `/Users/zb/workspace/uv`
DEBUG Project `uv` is marked as unmanaged
DEBUG No project found; searching for Python interpreter
DEBUG Reading Python requests from version file at `/Users/zb/workspace/uv/.python-version`
DEBUG Searching for Python 3.12 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.12.8-macos-aarch64-none` at `/Users/zb/workspace/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.8 interpreter at: /Users/zb/workspace/uv/.venv/bin/python3
DEBUG Running `python example.py`
PID: 84317
GPID: 84317
^CDEBUG Received SIGINT, forwarding to child at 84317
SIGINT
```

---

_Marked ready for review by @zanieb on 2025-01-28 17:16_

---

_Comment by @zanieb on 2025-01-28 17:38_

Test coverage pretty painful to add here.

---

_Review requested from @Gankra by @zanieb on 2025-01-28 18:32_

---

_Review requested from @charliermarsh by @zanieb on 2025-01-28 18:32_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/run.rs`:17 on 2025-01-28 19:44_

Ah now I get it. Thanks for explaining this here!

---

_Review comment by @BurntSushi on `crates/uv/src/commands/run.rs`:104 on 2025-01-28 19:47_

Does it make sense to log an error here if one occurs?

---

_Review comment by @BurntSushi on `crates/uv/src/commands/run.rs`:123 on 2025-01-28 19:49_

Does the signal actually get sent to the child? Or maybe my general question here is, will `^C` still work to terminate the child?

---

_@BurntSushi approved on 2025-01-28 19:50_

Nice comment. I buy this. I'm unsure about Windows though.

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:104 on 2025-01-28 19:56_

Probably yeah. This is the existing behavior so I prefer to retain but we could add logging in a separate patch.

---

_@zanieb reviewed on 2025-01-28 19:56_

---

_@zanieb reviewed on 2025-01-28 19:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:123 on 2025-01-28 19:57_

Yeah so this should retain our existing behavior — it's just moved from a platform-wide block to a Windows-specific block. The note is just that we can't implement the more complicated behavior we do above for Unix.

---

_@zanieb reviewed on 2025-01-28 19:58_

---

_Review comment by @zanieb on `crates/uv/src/commands/run.rs`:123 on 2025-01-28 19:58_

I also had Aria test this assumption in PowerShell and the initial change adding the Ctrl-C handling fixed various bugs.

---

_Merged by @zanieb on 2025-01-28 20:00_

---

_Closed by @zanieb on 2025-01-28 20:00_

---

_Branch deleted on 2025-01-28 20:00_

---

_@BurntSushi reviewed on 2025-01-28 20:04_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/run.rs`:123 on 2025-01-28 20:04_

Ah nice! On the ball.

---
