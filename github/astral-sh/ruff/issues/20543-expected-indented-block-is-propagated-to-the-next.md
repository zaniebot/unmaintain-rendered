---
number: 20543
title: "\"Expected indented block\" is propagated to the next cell"
type: issue
state: open
author: turbotimon
labels:
  - bug
  - notebook
assignees: []
created_at: 2025-09-23T17:50:28Z
updated_at: 2025-10-22T07:34:50Z
url: https://github.com/astral-sh/ruff/issues/20543
synced_at: 2026-01-07T13:12:16-06:00
---

# "Expected indented block" is propagated to the next cell

---

_Issue opened by @turbotimon on 2025-09-23 17:50_

The "Expected indented block" error is propagated to the next cell. Since cells contain completed parts of code, this error should not propagate:

<img width="924" height="500" alt="Image" src="https://github.com/user-attachments/assets/927eeaec-2285-46f0-b23f-6d03fac8082f" />



---

_Label `notebook` added by @MichaReiser on 2025-09-24 08:09_

---

_Comment by @MichaReiser on 2025-09-24 08:10_

Hmm. This might be tricky to fix because we parse the entire notebook as one concatenated string. 

It might parse each cell separately and patch up the offsets (or add a parsing mode that starts with a relative offset).

---

_Label `bug` added by @MichaReiser on 2025-09-24 08:10_

---

_Comment by @turbotimon on 2025-10-22 07:06_

@MichaReiser That would be a nice fix though and helpful for the user, as the error message is shown at the cell where the error happened.

Maybe there could be a marker (e.g. newline) in the string and certain checks stop at that marker?  But perhaps that is a naive idea, as I don't know any details about the current implementation.

If scope spans over cells, it leads to other wrong error message like this where the next cell is considered still part of the function:

<img width="841" height="358" alt="Image" src="https://github.com/user-attachments/assets/19ed7a18-b9b5-475a-b6c4-a7854a9d16bd" />


Just for reference, **Pylance** does it correct and reports two separate issues:

<img width="743" height="246" alt="Image" src="https://github.com/user-attachments/assets/256a2d1c-317a-41f3-b283-cca37d80ea94" />

<img width="571" height="205" alt="Image" src="https://github.com/user-attachments/assets/2922cdc0-a9aa-418a-aa9c-6e573f7fef33" />


---
