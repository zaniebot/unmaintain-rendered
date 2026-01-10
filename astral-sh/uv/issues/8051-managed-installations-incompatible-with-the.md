---
number: 8051
title: "managed installations - incompatible with the project's Python requirement `>=3.13`"
type: issue
state: open
author: masiarek
labels:
  - question
assignees: []
created_at: 2024-10-09T18:02:26Z
updated_at: 2024-10-09T21:01:49Z
url: https://github.com/astral-sh/uv/issues/8051
synced_at: 2026-01-10T01:24:23Z
---

# managed installations - incompatible with the project's Python requirement `>=3.13`

---

_Issue opened by @masiarek on 2024-10-09 18:02_

This is most likely my mac set-up issue - not a bug - but maybe you could guide me how to fix it?
I tried Python 3.12 as well - the same symptom...
'''
amasa@Adams-iMac my_p3 % uv sync --verbose
DEBUG uv 0.4.20
DEBUG Found project root: `/Volumes/T7/_trash/my_p3`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Volumes/T7/_trash/my_p3/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.11`
DEBUG The virtual environment's Python version does not meet the project's Python requirement: `>=3.13`
DEBUG Searching for Python 3.11 in managed installations or system path
DEBUG Searching for managed installations at `/Volumes/T5/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.12.7-macos-x86_64-none`
DEBUG Found managed installation `cpython-3.11.10-macos-x86_64-none`
DEBUG Found `cpython-3.11.10-macos-x86_64-none` at `/Volumes/T5/.local/share/uv/python/cpython-3.11.10-macos-x86_64-none/bin/python3` (managed installations)
Using CPython 3.11.10
error: The Python request from `.python-version` resolved to Python 3.11.10, which is incompatible with the project's Python requirement: `>=3.13`
'''

---

_Comment by @zanieb on 2024-10-09 18:08_

I think you wanted to create your project with a different `requires-python`, e.g. `requires-python = ">=3.11"` or `uv init --python 3.11` — or, you want to change your pin, e.g. `uv python pin 3.13`

---

_Label `question` added by @zanieb on 2024-10-09 18:08_

---

_Comment by @masiarek on 2024-10-09 21:01_

yes, pinning (uv python pin 3.13) - solved the issue.
Video: 
https://youtu.be/hVXmQWQ-_I0

<img width="932" alt="image" src="https://github.com/user-attachments/assets/0182aa12-3119-4fe6-8ef7-b3e76f1c6d20">


---
