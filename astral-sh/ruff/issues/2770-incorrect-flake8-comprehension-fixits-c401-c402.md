```yaml
number: 2770
title: Incorrect flake8-comprehension fixits (C401,C402) in fstring
type: issue
state: closed
author: Skylion007
labels:
  - bug
assignees: []
created_at: 2023-02-11T16:42:52Z
updated_at: 2023-02-12T16:03:39Z
url: https://github.com/astral-sh/ruff/issues/2770
synced_at: 2026-01-10T11:09:45Z
```

# Incorrect flake8-comprehension fixits (C401,C402) in fstring

---

_Issue opened by @Skylion007 on 2023-02-11 16:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
One annoying false positive change in this flake8-comprehensions are set/dict conversions to literals in f'strings.

```python
b = list(range(10))
print(f'{set(a if a < 6 else 0  for a in b)}')
```
which prints: `{0, 1, 2, 3, 4, 5}`

becomes

```python
b = list(range(10))
print(f'{{a if a < 6 else 0  for a in b}}')
```
which is actually wrong if it's in a fstring since it actually just escapes the innermost curly brace. This results in the printing of `{a if a < 6 else 0 for a in b}`

It should fixit as 

```python
b = list(range(10))
print(f'{ {a if a < 6 else 0  for a in b} }')
```
The space will prevent the curly brace from being escaped and is deleted when the fstring resolves which should provide identical behavior to the original code snippit. See this Stackoverflow post: https://stackoverflow.com/questions/49120113/dictionary-set-comprehensions-inside-of-f-string

I suspect you wound find similar behavior for C402 with dict comprehensions.

Command, Version Info, and Settings
```
ruff --select C401,C402 example.py --fix
ruff 0.0.244
no ruff pyproject.toml ruff settings specified
```

---

_Label `bug` added by @charliermarsh on 2023-02-11 16:58_

---

_Comment by @charliermarsh on 2023-02-11 16:58_

Ahh thank you! Good catch. I guess arguably we should avoid flagging this at all in f-strings.

---

_Comment by @charliermarsh on 2023-02-11 16:58_

\cc @sbrugman if you're interested.

---

_Comment by @Skylion007 on 2023-02-11 17:15_

@charliermarsh It's not a bad change in fstrings, and it is potentially more performant, just need to be careful to do it properly :P . Another option is just to add an extra space around the inserted braces unconditionally and let black or whatever formatter you use deal with it.

---

_Comment by @charliermarsh on 2023-02-11 17:17_

Yeah I could see either being "correct". Regardless sorry for the breakage!

---

_Closed by @charliermarsh on 2023-02-12 16:03_

---
