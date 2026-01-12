```yaml
number: 10986
title: Fish Shell Activation Script Compatibility Issue
type: issue
state: closed
author: CedricRaison
labels:
  - question
assignees: []
created_at: 2025-01-27T14:41:33Z
updated_at: 2025-01-27T19:24:49Z
url: https://github.com/astral-sh/uv/issues/10986
synced_at: 2026-01-12T16:00:25Z
```

# Fish Shell Activation Script Compatibility Issue

---

_@CedricRaison_

### Summary

# Fish Shell Activation Script Compatibility Issue

## Description
When attempting to activate a Python virtual environment created with `uv` in the Fish shell, the activation script fails due to Bash-specific syntax that is incompatible with Fish.

## Steps to Reproduce

1. Install and pin Python 3.11 in your project:
   ```bash
   uv python install 3.11
   uv python pin 3.11
   ```

2. Create virtual environment with uv:
   ```bash
   uv venv .venv
   ```

3. Try to activate the environment in Fish shell:
   ```fish
   source .venv/bin/activate
   ```

## Error Message
```
source .venv/bin/activate
.venv/bin/activate (line 27): Unsupported use of '='. In fish, please use 'set SCRIPT_PATH "${BASH_SOURCE[0]}"'.
    SCRIPT_PATH="${BASH_SOURCE[0]}"
    ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^
from sourcing file .venv/bin/activate
source: Error while reading file '.venv/bin/activate'
```

## Environment
- Shell: Fish
- Python Version: 3.11
- Package Manager: uv
- Operating System: Ubuntu 24.04

## Expected Behavior
The virtual environment should activate successfully when using the `source .venv/bin/activate` command in Fish shell.

## Current Behavior
The activation fails because the script uses Bash-specific syntax (`BASH_SOURCE[0]`) which is not compatible with Fish shell's syntax requirements.

## Possible Solution
The activation script should either:
1. Detect the current shell and use appropriate syntax
2. Provide a separate Fish-compatible activation script (e.g., `activate.fish`)
3. Update the current script to use Fish-compatible syntax: `set SCRIPT_PATH (status filename)`

## Additional Context
This issue affects users who prefer Fish shell as their default shell environment. Currently, they cannot activate virtual environments created by uv without manual workarounds.

## Workaround
Until this is fixed, users can use a different shell (bash/zsh) for activating the virtual environment.





### Platform

ubuntu 24.04

### Version

uv 0.5.23

### Python version

3.11

---

_Label `bug` added by @CedricRaison on 2025-01-27 14:41_

---

_Comment by @FishAlchemist on 2025-01-27 18:02_

Is there no ``activate.fish`` file in the ``bin`` directory? If there is, you should be able to use the following command.
```bash
source .venv/bin/activate.fish
```

https://docs.python.org/3/library/venv.html#how-venvs-work

![Image](https://github.com/user-attachments/assets/da26eb0c-a880-4bdc-9b7b-d13156da689a)

---

_Label `bug` removed by @zanieb on 2025-01-27 18:22_

---

_Label `question` added by @zanieb on 2025-01-27 18:22_

---

_Comment by @CedricRaison on 2025-01-27 18:39_

Yes there is, thanks a lot, I did not know it exists.

https://github.com/astral-sh/uv/blob/90a4178c7a8fde7fabae25ea97b5e5879ba165a0/docs/pip/environments.md

There is no mention of this in the uv documentation.

I will try to make a PR to add this.

---

_Closed by @zanieb on 2025-01-27 19:24_

---

_Closed by @zanieb on 2025-01-27 19:24_

---
