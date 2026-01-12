```yaml
number: 8006
title: "fix: Improve compatibility with VSCode PS1 prompt"
type: pull_request
state: merged
author: quodlibetor
labels:
  - bug
assignees: []
merged: true
base: main
head: quodlibetor/improve-venv-vscode-compat
created_at: 2024-10-08T15:47:10Z
updated_at: 2024-10-09T15:47:43Z
url: https://github.com/astral-sh/uv/pull/8006
synced_at: 2026-01-12T16:08:07Z
```

# fix: Improve compatibility with VSCode PS1 prompt

---

_@quodlibetor_

## Summary

This was previously reported in #3037, and the repro there still works -- create any venv using uv and the prompt will be `nameoriginalps1` instead of `(name) originalps1`. VSCode fixed one of the two bugs reported there.

The VSCode-generated activate script is pretty different from the uv-generated activate script. The most important thing seems to be that vscode's prompt takes the VIRTUAL_ENV_PROMPT directly instead of obeying PS1, from VSCodes activate script:

```shell
if [ -z "${VIRTUAL_ENV_DISABLE_PROMPT:-}" ] ; then
    _OLD_VIRTUAL_PS1="${PS1:-}"
    PS1="(.venv) ${PS1:-}"
    export PS1
    VIRTUAL_ENV_PROMPT="(.venv) "
    export VIRTUAL_ENV_PROMPT
fi
```

This changes uv to use VIRTUAL_ENV_PROMPT directly, which does fix vscode for me.

This changes uv to be different from [what pypa/virtualenv does][1], so I'm not really sure if it should be accepted or if a bug should be reported to VSCode.

[1]: https://github.com/pypa/virtualenv/blob/b56d09248aecf584e985956185afb6745a7daac7/src/virtualenv/activation/bash/activate.sh#L71-L75

## Test Plan

I made the equivalent change in the uv-generated activate script, closed my vscode terminal, cmd-shift-p "reload window", and reopened a terminal and observed it to work. Note that both closing the terminal and reloading the window seem to be required.

Also I have the `python.terminal.activateEnvironment` setting enabled and `python.terminal.activateEnvInCurrentTerminal` disabled (I didn't even know about the second one until right now).


---

_Assigned to @konstin by @zanieb on 2024-10-08 17:41_

---

_Review requested from @konstin by @zanieb on 2024-10-08 17:41_

---

_@konstin approved on 2024-10-09 15:33_

Since this is only an aesthetic change, I'm fine with deviating from pypa/virtualenv here

---

_Label `bug` added by @konstin on 2024-10-09 15:33_

---

_Merged by @konstin on 2024-10-09 15:33_

---

_Closed by @konstin on 2024-10-09 15:33_

---

_Branch deleted on 2024-10-09 15:47_

---
