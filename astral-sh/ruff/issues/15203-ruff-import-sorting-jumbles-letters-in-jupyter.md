```yaml
number: 15203
title: Ruff import sorting jumbles letters in jupyter notebook
type: issue
state: closed
author: hhalaby
labels:
  - question
assignees: []
created_at: 2024-12-30T15:21:25Z
updated_at: 2024-12-30T16:11:24Z
url: https://github.com/astral-sh/ruff/issues/15203
synced_at: 2026-01-10T11:09:56Z
```

# Ruff import sorting jumbles letters in jupyter notebook

---

_Issue opened by @hhalaby on 2024-12-30 15:21_

## Description
Automatic sorting of imports on save in jupyter notebook using Ruff leads to very weird behavior and the result is words and letters jumbled up together.

## Example
Here is a sample video

https://github.com/user-attachments/assets/39003caa-895e-48db-ae18-967c8947c419


If I add an import line that should be higher up, Ruff completely messes up the words and letters.
The second time I add the same line twice and Ruff ends up deleting another line entirely.

## Additional info

- **Sorting imports in an empty notebook that only contains one imports cell does not trigger the weird behavior**

- VSCode settings with issue:

```
    "[python]": {
        "editor.defaultFormatter": "charliermarsh.ruff"
    },
    "ruff.organizeImports": true,
    "notebook.codeActionsOnSave": {
        "source.organizeImports": "explicit"
    },
```

- ruff extension is installed - Version: 2024.56.0
- isort extention is not installed


---

_Comment by @dhruvmanila on 2024-12-30 15:55_

Can you try using `notebook.source.organizeImports` instead? We recommend using that (https://github.com/astral-sh/ruff-vscode#configuring-vs-code).

---

_Label `question` added by @dhruvmanila on 2024-12-30 15:55_

---

_Comment by @dhruvmanila on 2024-12-30 15:57_

I briefly explained it here: https://github.com/astral-sh/ruff-vscode/issues/640#issuecomment-2462210318

---

_Comment by @hhalaby on 2024-12-30 16:07_

Thanks a lot ! 
For what it's worth I actually tried searching in the issue tracker but I guess I didn't use the right keywords.. sorry for the bother <3 

---

_Closed by @hhalaby on 2024-12-30 16:07_

---

_Comment by @dhruvmanila on 2024-12-30 16:11_

> For what it's worth I actually tried searching in the issue tracker but I guess I didn't use the right keywords.. sorry for the bother <3

No worries, GitHub issues can sometimes be a bit difficult to search if you don't know the correct keywords :)

---
