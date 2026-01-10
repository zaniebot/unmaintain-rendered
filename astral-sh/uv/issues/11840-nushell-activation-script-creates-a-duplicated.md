---
number: 11840
title: nushell activation script creates a duplicated prompt
type: issue
state: closed
author: Ravencentric
labels:
  - bug
assignees: []
created_at: 2025-02-27T21:33:16Z
updated_at: 2025-02-28T11:30:34Z
url: https://github.com/astral-sh/uv/issues/11840
synced_at: 2026-01-10T01:25:11Z
---

# nushell activation script creates a duplicated prompt

---

_Issue opened by @Ravencentric on 2025-02-27 21:33_

### Summary

 > I initially opened this in the nushell repository (https://github.com/nushell/nushell/issues/15205) but was told to bring it up here.

---

nu:

![Image](https://github.com/user-attachments/assets/8e706bd4-58cb-4866-8336-062bb39c7b39)

Notice the duplicated `(.venv)`.

pwsh:

![Image](https://github.com/user-attachments/assets/ac4b78e7-4d82-41cc-9a5b-028755ee30ec)


### How to reproduce

1. Install [starship](https://starship.rs/guide/)
2. Add the following to `$nu.config-path`:
    ```nu
    mkdir ($nu.data-dir | path join "vendor/autoload")
    starship init nu | save -f ($nu.data-dir | path join "vendor/autoload/starship.nu")
    ```
3. Create a venv with [uv](https://docs.astral.sh/uv/): `uv venv`
4. Activate the venv with `overlay use .venv/Scripts/activate.nu`


### Platform

Windows 11 x86_64

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

Python 3.13.0

---

_Label `bug` added by @Ravencentric on 2025-02-27 21:33_

---

_Comment by @cptpiepmatz on 2025-02-27 23:57_

I don't think that is an issue here, the `VIRTUAL_ENV_DISABLE_PROMPT` env variable already allows to remove that extra line.

I also explained it more [here](https://github.com/nushell/nushell/issues/15205#issuecomment-2689373775).

---

_Comment by @Ravencentric on 2025-02-28 11:30_

That did the trick, thank you.

---

_Closed by @Ravencentric on 2025-02-28 11:30_

---
