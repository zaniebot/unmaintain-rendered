---
number: 6353
title: "Better help for `uv run --help`"
type: issue
state: closed
author: simonw
labels:
  - question
assignees: []
created_at: 2024-08-21T16:37:30Z
updated_at: 2024-09-05T00:29:28Z
url: https://github.com/astral-sh/uv/issues/6353
synced_at: 2026-01-10T01:23:59Z
---

# Better help for `uv run --help`

---

_Issue opened by @simonw on 2024-08-21 16:37_

Currently:
```bash
uv run --help
```
Output:
```
Run a command or script

Usage: uv run [OPTIONS] <COMMAND>

Options:
      --extra <EXTRA>
          Include optional dependencies from the extra group name
...
```
This isn't enough information. I want to understand how this differs from running the command directly. I was hoping to learn if it would automatically behave as if I had activated my `.venv/` virtual environment, for example.

---

_Comment by @zanieb on 2024-08-21 16:44_

At the bottom, we suggest `uv run help` to see the long-form help.

---

_Label `question` added by @zanieb on 2024-08-21 16:44_

---

_Comment by @gulbanana on 2024-08-22 11:40_

that doesn't work for me - it invokes the "help" command instead

```
banana@valiarde MINGW64 ~/Documents/code/uv-test
$ uv run help
   Built uv-test @ file:///C:/Users/banana/Documents/code/uv-test
Uninstalled 1 package in 1ms
Installed 1 package in 6ms
For more information on a specific command, type HELP command-name
ASSOC          Displays or modifies file extension associations.
ATTRIB         Displays or changes file attributes.
BREAK          Sets or clears extended CTRL+C checking.
BCDEDIT        Sets properties in boot database to control boot loading.
CACLS          Displays or modifies access control lists (ACLs) of files.
CALL           Calls one batch program from another.
CD             Displays the name of or changes the current directory.
CHCP           Displays or sets the active code page number.
CHDIR          Displays the name of or changes the current directory.
CHKDSK         Checks a disk and displays a status report.
```
&c

---

_Comment by @zanieb on 2024-08-22 12:57_

Sorry, `uv help run` — `uv help` is the interface for the CLI reference.

---

_Closed by @zanieb on 2024-09-05 00:29_

---
