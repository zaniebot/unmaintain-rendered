---
number: 16484
title: "Ruff's VSCode extension doesn't recongnize terminal command executions in jupyter notebooks"
type: issue
state: closed
author: juanbfactored
labels:
  - question
assignees: []
created_at: 2025-03-03T21:47:57Z
updated_at: 2025-03-04T18:20:52Z
url: https://github.com/astral-sh/ruff/issues/16484
synced_at: 2026-01-07T13:12:16-06:00
---

# Ruff's VSCode extension doesn't recongnize terminal command executions in jupyter notebooks

---

_Issue opened by @juanbfactored on 2025-03-03 21:47_

### Summary

The ruff linter VS Code extension assigns syntax errors to any line using the terminal magic of "!" in any Jupyter notebook cell. For example, if I have a cell like this:

`!brew install ruff`

The extension will highlight this line and tell me I have a syntax error. For more reference see the image below

<img width="560" alt="Image" src="https://github.com/user-attachments/assets/9b4cff56-b873-4880-8e5d-dfa74b0ee6bf" />

### Version

2025.14.0

---

_Comment by @ntBre on 2025-03-04 00:12_

Is this in an unsaved file/buffer possibly? I'm looking at the filename `ruff_demo.ipynb#.....` and thinking the bit after `#` may be confusing our file type detection.

I'm not able to reproduce this with a regular filename like `try.ipynb`.

---

_Label `question` added by @ntBre on 2025-03-04 00:12_

---

_Comment by @juanbfactored on 2025-03-04 15:11_

It is not an unsaved file or buffer. I cloned a repo and opened this notebook in VS Code. To make sure it wasn't related to any issues with the file, I closed the tab with the notebook, quit VS Code, and reopened it. The error is still there.

Additionally, I created another notebook `test.ipynb` and the same incorrect error was highlighted when I opened it in VS Code.

<img width="448" alt="Image" src="https://github.com/user-attachments/assets/52544cad-de21-43d9-a15b-b7259d1e4b43" />

---

_Comment by @dhruvmanila on 2025-03-04 16:33_

Hmm, that's unexpected. I think that diagnostic might not be coming from Ruff because we don't have any message that says "SyntaxError: invalid syntax (<filename>)" or at least I can't find it.

Can you provide the entire message? It should end with "Ruff" like in the screenshot below. If it doesn't, then it might be some other extension that's causing it.

<img width="964" alt="Image" src="https://github.com/user-attachments/assets/5cd2c345-b6ed-4736-aee1-7881268db200" />

---

_Comment by @juanbfactored on 2025-03-04 18:20_

I don't have Pylance installed but when I did the error message went away. I don't know what is happening but it seems VS Code is checking for "syntax errors" with some other extension or functionality that may come from the Python extension. Anyway, thank you for taking a look at this and I'll be leaving Pylance installed to avoid these errors from showing up.

---

_Closed by @juanbfactored on 2025-03-04 18:20_

---
