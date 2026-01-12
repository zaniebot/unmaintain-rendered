```yaml
number: 16623
title: Noise vscode error card every time I open a new window
type: issue
state: open
author: jpambrun
labels:
  - needs-info
assignees: []
created_at: 2025-03-11T12:25:45Z
updated_at: 2025-03-11T17:11:06Z
url: https://github.com/astral-sh/ruff/issues/16623
synced_at: 2026-01-12T15:54:55Z
```

# Noise vscode error card every time I open a new window

---

_@jpambrun_

### Summary

Every time I open a new vscode window I get a error card on the right bottom. Ruff does work, but it's really annoying. The "problem" is that I don't have ruff in the venv used by vscode, but I am happy with that. I want to use the bundled version without the needless noise. 

Here are the logs:
```
2025-03-11 08:13:11.613 [info] Name: Ruff
2025-03-11 08:13:11.613 [info] Module: ruff
2025-03-11 08:13:11.613 [info] Python extension loading
2025-03-11 08:13:11.613 [info] Waiting for interpreter from python extension.
2025-03-11 08:13:11.619 [info] Python extension loaded
2025-03-11 08:13:11.622 [info] Using interpreter: /home/jpambrun/work/ml-serve/applications/orchestrator/.venv/bin/python
2025-03-11 08:13:11.634 [error] Error while trying to find the Ruff binary: Error: spawn /home/jpambrun/work/ml-serve/applications/orchestrator/.venv/bin/python ENOENT
2025-03-11 08:13:15.699 [info] Falling back to bundled executable: /home/jpambrun/.vscode-server/extensions/charliermarsh.ruff-2025.14.0-linux-x64/bundled/libs/bin/ruff
2025-03-11 08:13:15.724 [info] Resolved 'ruff.nativeServer: auto' to use the native server
2025-03-11 08:13:15.728 [info] Found Ruff 0.9.9 at /home/jpambrun/.vscode-server/extensions/charliermarsh.ruff-2025.14.0-linux-x64/bundled/libs/bin/ruff
2025-03-11 08:13:15.728 [info] Server run command: /home/jpambrun/.vscode-server/extensions/charliermarsh.ruff-2025.14.0-linux-x64/bundled/libs/bin/ruff server
2025-03-11 08:13:15.728 [info] Server: Start requested.
```

### Version

_No response_

---

_Comment by @dhruvmanila on 2025-03-11 16:49_

Hmm, that's unexpected. Are you by any chance using the [`ruff.interpreter`](https://docs.astral.sh/ruff/editors/settings/#interpreter) option? Do you mind providing me with your VS Code settings related to Ruff?

---

_Label `needs-info` added by @dhruvmanila on 2025-03-11 16:49_

---

_Comment by @jpambrun on 2025-03-11 17:11_

I didn't have `ruff.interpreter` set (I only had lint.select/ignore and configurationPreference to filesystemFirst), but adding 

```json
  "ruff.interpreter": [
    "/usr/sbin/python"
  ]
```
did make the error go away. Now the logs are 

```
2025-03-11 13:07:03.513 [info] Name: Ruff
2025-03-11 13:07:03.513 [info] Module: ruff
2025-03-11 13:07:03.513 [info] Using interpreter: /usr/sbin/python
2025-03-11 13:07:03.679 [info] Falling back to bundled executable: /home/jpambrun/.vscode-server/extensions/charliermarsh.ruff-2025.14.0-linux-x64/bundled/libs/bin/ruff
2025-03-11 13:07:03.712 [info] Resolved 'ruff.nativeServer: auto' to use the native server
2025-03-11 13:07:03.717 [info] Found Ruff 0.9.9 at /home/jpambrun/.vscode-server/extensions/charliermarsh.ruff-2025.14.0-linux-x64/bundled/libs/bin/ruff
2025-03-11 13:07:03.717 [info] Server run command: /home/jpambrun/.vscode-server/extensions/charliermarsh.ruff-2025.14.0-linux-x64/bundled/libs/bin/ruff server
2025-03-11 13:07:03.718 [info] Server: Start requested.
```

---
