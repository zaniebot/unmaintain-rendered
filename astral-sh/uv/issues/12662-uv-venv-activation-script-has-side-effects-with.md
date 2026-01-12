```yaml
number: 12662
title: "`uv venv` activation script has side effects with zsh and bash"
type: issue
state: closed
author: zsimic
labels:
  - bug
assignees: []
created_at: 2025-04-04T03:17:24Z
updated_at: 2025-04-07T18:11:48Z
url: https://github.com/astral-sh/uv/issues/12662
synced_at: 2026-01-12T16:01:09Z
```

# `uv venv` activation script has side effects with zsh and bash

---

_@zsimic_

### Summary

The `venv/bin/activate` script has an unfortunate side-effect: it defines and leaves behind a variable called `SCRIPT_PATH`, which can then adversely impact shell scripts that also had the idea of using a `SCRIPT_PATH` shell variable.

Ideally, the activation script should not leak variables, or as few as possible.

I'm attaching here a script that shows what happens, and the difference with standard `-mvenv` activation script.

You can observe this issue by running this demo shell script in a sandbox folder somewhere (it creates 3 files):
```
zsh repro-venv-leak.sh     # no leak with -mvenv
zsh repro-venv-leak.sh uv  # leak with uv

# Same with bash:
bash repro-venv-leak.sh
bash repro-venv-leak.sh uv
```


Script:
```
#!/bin/bash

record_shell_variables() {
    local _where=$1
    set | grep -Ev "^(_|PS1|_OLD_VIRTUAL_PS1|(BASH_)LINENO|RANDOM|'\*'|@|_where|argv)=" > "$_where"
}

if [[ "$1" == "uv" ]]; then
    echo "Creating venv with uv"
    uv venv
else
    echo "Creating venv with python3 -mvenv"
    rm -rf .venv ; python3 -mvenv .venv
fi

record_shell_variables original-vars.txt
echo "Activating then de-activating venv"
source .venv/bin/activate
record_shell_variables activated-vars.txt
deactivate
record_shell_variables post-activation-vars.txt

echo "Diff (should be empty):"
diff original-vars.txt post-activation-vars.txt
echo --------
```

### Platform

macos, linux

### Version

0.6.12

### Python version

any

---

_Label `bug` added by @zsimic on 2025-04-04 03:17_

---

_Comment by @zsimic on 2025-04-04 03:26_

It appears that the shell variable `SCRIPT_PATH` is not used in the activation script, but is used from outside, to try and make venvs relocatable.

May I suggest to rename the variable `_UV_VENV_ACTIVATOR`? This will lower the chance of clashing with other shell scripts out in the wild that can be surprised by this "side effect"

---

_Comment by @charliermarsh on 2025-04-04 12:50_

That seems like a good idea.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-04-04 12:54_

---

_Closed by @zanieb on 2025-04-07 18:11_

---
