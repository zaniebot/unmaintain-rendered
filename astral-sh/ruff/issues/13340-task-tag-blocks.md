---
number: 13340
title: Task Tag Blocks
type: issue
state: closed
author: scarere
labels: []
assignees: []
created_at: 2024-09-12T19:24:01Z
updated_at: 2024-09-13T22:50:17Z
url: https://github.com/astral-sh/ruff/issues/13340
synced_at: 2026-01-10T01:22:53Z
---

# Task Tag Blocks

---

_Issue opened by @scarere on 2024-09-12 19:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
I've really enjoyed using ruff recently. One feature that I like is the option to ignore linting on task tags and the ability to define custom task tags. I noticed recently that task tags are only recognized if they are in a comment, but not a triple quoted comment block. Eg.
```python
# TODO: E501 surpressed on this comment when ignore-overlong-task-comments = true

"""
TODO: E501 not surpressed on this comment even when ignore-overlong-task-comments = true
"""
```

I'm not sure if this is a bug, or if it is the intended behaviour. Either way I think it is not an uncommon desire to want to write out more detailed TODO notes that span several lines. My typical solution is to put them in triple quoted comment blocks but then as shown above, they will not be detected by ruff.

I proposed two changes:
1. Make ruff recognize task tags that are within triple quoted strings
2. Specify a way to designate a block of comment lines with a task tag so that the tag itself doesn't have to be repeated.

Example of what change 2 might look like
```python
"""
Maybe use indentation
TODOLIST: Add som new feature to module
    - indented item automatically marked as a TODO item. E501 ignored when ignore-overlong-task-comments = true
Unindenting ends the todo list
"""

# Or maybe use a new special tag
# ruff: start TODO
# Comments in between start and end tags ignore E501 when ignore-overlong-task-comments = true
# ruff: end TODO
```


---

_Comment by @MichaReiser on 2024-09-13 14:02_

Hi @scarere 

This behavior is intentional because multiline docstrings aren't considered comments in the Python specification. They're strings. Some editors support multiline comments by indenting the next line like this:


```python
# TODO: We should make this work on windows
#  The current logic doesn't account for windows.
#  more content...
```



---

_Comment by @scarere on 2024-09-13 15:09_

Hi @MichaReiser 

That would be a useful workaround, does VS Code have an extension for this?

---

_Comment by @MichaReiser on 2024-09-13 17:17_

Hmm not sure. I don't use VS code but I quickly tried using TODO comments and it seems that VS code doesn't use any special syntax highlighting for anything other than the TODO keyword itself. So it should just work?

---

_Closed by @MichaReiser on 2024-09-13 22:50_

---
