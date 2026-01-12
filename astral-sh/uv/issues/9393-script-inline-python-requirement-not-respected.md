```yaml
number: 9393
title: Script inline python requirement not respected when .python-version file is in project
type: issue
state: open
author: Magglish
labels:
  - question
  - needs-decision
assignees: []
created_at: 2024-11-24T10:30:00Z
updated_at: 2025-09-28T15:40:23Z
url: https://github.com/astral-sh/uv/issues/9393
synced_at: 2026-01-12T15:59:49Z
```

# Script inline python requirement not respected when .python-version file is in project

---

_@Magglish_

Hello,

I'm really exicted that we can specify python version and libraries dependency for scripts in `uv` and this thing can convinced me to move from `poetry` to `uv`.

**Description**

Unfortunately, I've encounter an issue which I don't if it's a bug or expected behaviour.
The problem is that script inline python requirement is not respected by `uv` when `.python-version` file is specified in the same directory.

**Steps to reproduce:**

1. OS Version: Ubuntu 22.04 LTS
2. `uv` version: **0.5.4** (installed through official installation script as mentioned in documentation)

3. `.python-version` contains:

```
3.12.7
```

4. `example.py` script with dependencies:

```python
# /// script
# requires-python = "==3.11.5"
# dependencies = [
#   "rich==13.9.4",
# ]
# ///

import sys
import time
from rich.progress import track

for i in track(range(20), description="For example:"):
    time.sleep(0.05)
print(sys.version)
```

5. Create venv with `uv`

```bash
uv venv
```

6. Run `example.py`

```bash
uv run example.py
```

What we see:

1. warning: The Python request from `.python-version` resolved to Python 3.12.7, which is incompatible with the script's Python requirement: `==3.11.5`
2. `print(sys.version)` returns `3.12.7` but not `3.11.5`

What should we see:

The script should be run with python 3.11.5, but it's not.

**Alternative**

We can run `example.py` with

```bash
uv run --python 3.11.5 example.py
```

But this means that the inline metadata requirements are not being used at all.

Is this a bug or a feature?

---

_Comment by @FishAlchemist on 2024-11-24 10:57_

It has a warning, so I personally think it's an expected behavior.
```
warning: The Python request from `.python-version` resolved to Python 3.12.7, which is incompatible with the script's Python requirement: `==3.11.5`
```
Personally, I believe that if the script has a specified version, we ought to adhere to it unless a different version is explicitly defined via the CLI.
However, since uv run is categorized as a project API, it is also reasonable for it to respect the .python-version file under the project.

---

_Comment by @Magglish on 2024-11-24 12:02_

In my opinion, if the `uv` wants to respect the inline metadata requirements presented in the scripts then all requirements should be respected. 

---

_Label `question` added by @charliermarsh on 2024-11-24 16:29_

---

_Comment by @charliermarsh on 2024-11-24 16:29_

Yeah I believe this is intentional. Another option would be to throw a hard error because you have conflicting requirements.

---

_Comment by @zanieb on 2024-11-25 23:33_

I think if the `.python-version` conflicts with a script's Python requirement, we should prefer the script's definition. This is consistent with projects — which won't use a `.python-version` file from a parent directory if it conflicts with their requirement. Since scripts are intended to be self-contained, I think all the `.python-version` pins could be considered as from a "parent".

---

_Label `needs-decision` added by @zanieb on 2024-11-25 23:35_

---

_Comment by @charliermarsh on 2024-11-25 23:35_

So in that case, we would respect `.python-version` if it's consistent, and ignore it otherwise? (Not disagreeing, just confirming.)

---

_Comment by @zanieb on 2024-11-26 00:33_

Yeah, that's the suggestion. I think we'd at least want to log that we were ignoring it though.

> This is consistent with projects — which won't use a `.python-version` file from a parent directory if it conflicts with their requirement. 

This is actually wrong. I previously implemented this behavior, but then we decided to stop `.python-version` file discovery at the workspace boundary which eliminated this complexity.

I'm not sure if this changes my mind, but it definitely lowers my certainty. I feel like `.python-version` is more project-focused than script-focused, so my intuition is that we should reverse the existing behavior.

For reference, the current warning is implemented at https://github.com/astral-sh/uv/blob/5759cb9891c0d3ca8dca67f7518e7e439018beea/crates/uv/src/commands/project/run.rs#L212-L225

---

_Comment by @Magglish on 2024-12-09 07:25_

@zanieb did your team had a chance to talk about this issue and decide? Is there a chance that you will revert the implementation and the script inline requirements for python version will be respected? :) 

---

_Comment by @Magglish on 2024-12-19 11:37_

Bump

---

_Comment by @ckutlu on 2024-12-29 18:45_

Bump.

I ran into this while writing a CLI helper for a library I'm working on. The library has to support python 3.9 while the CLI helper, being completely decoupled from the project apart from being hosted in the same repo, needs to be >3.9 because of a bug in one of its dependencies for older python versions.

---

_Comment by @coezbek on 2025-09-28 15:39_

The documentation on this is currently misleading (see https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies). It says:

```
uv also respects Python version requirements:

example.py

# /// script
# requires-python = ">=3.12"
# dependencies = []
# ///

# Use some syntax added in Python 3.12
type Point = tuple[float, float]
print(Point)

Note: The dependencies field must be provided even if empty.

uv run will search for and use the required Python version. The Python version will download if it is not 
installed — see the documentation on [Python versions](https://docs.astral.sh/uv/concepts/python-versions/) 
for more details.
```

As far as I can see, `uv` doesn't search and use the required Python version, but rather it uses the version specified in `.python-version` in the current or all parent folders and just throws a warning if there is a mismatch.

If `.python-version` is missing then the resolution works as expected and documented.

Maybe to summarize what I think should be the requirements of python version resolution when running scripts:

- If a script contains a version requirement, then this requirement is fulfilled or the run is aborted.
- If a `.python-version` is available and it matches the `requires-python`, then this version of python is used to run the script.
- If no `.python-version` is available, the lowest (?) version fulfilling the requirement which is already installed is used.
- If no python is installed fulfilling the requirement, `uv` will download the lowest (?) version fulfilling the `requires-python` version.

---
