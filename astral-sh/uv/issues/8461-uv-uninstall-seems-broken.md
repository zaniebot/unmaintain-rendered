---
number: 8461
title: uv uninstall seems broken
type: issue
state: closed
author: ArkashJ
labels:
  - question
assignees: []
created_at: 2024-10-22T14:43:37Z
updated_at: 2024-10-22T23:55:22Z
url: https://github.com/astral-sh/uv/issues/8461
synced_at: 2026-01-10T01:24:29Z
---

# uv uninstall seems broken

---

_Issue opened by @ArkashJ on 2024-10-22 14:43_

I uninstalled uv from my machine, deleted it from my bashrc and cleared my cache however I still get some issues. 

1) I wanted to uninstall and reinstall uv locally because `uv add` and `uv sync` were failing however `uv pip install` was working. I tried running `uv cache clean`, `uv cache prune`, `uv self updated` however all these commands with fail saying command not found.
2) I decided to follow this stack overflow article to uninstall uv (https://stackoverflow.com/questions/78076401/uninstall-uv-python-package-installer) and it didnt work.
3) I did a pip uninstall of uv but i expected it to fail since i got it via brew
4) I ran the commands mentioned on the official uv page and even went to the locations to verify no uv, however I can still run uv in my shell.

Any thoughts?

<img width="808" alt="Screenshot 2024-10-22 at 10 43 08 AM" src="https://github.com/user-attachments/assets/89b6afbe-e6af-4daf-a076-fe041b014d23">


---

_Comment by @charliermarsh on 2024-10-22 14:52_

What does `which uv` return?

---

_Label `question` added by @charliermarsh on 2024-10-22 14:52_

---

_Comment by @ArkashJ on 2024-10-22 16:38_

`/opt/homebrew/bin/uv`

---

_Comment by @charliermarsh on 2024-10-22 21:33_

I think you have to uninstall with Homebrew, or even just remove the binary (`rm -rf /opt/homebrew/bin/uv`).

---

_Closed by @charliermarsh on 2024-10-22 23:55_

---
