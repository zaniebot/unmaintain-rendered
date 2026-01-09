---
number: 14841
title: "Does `uv python pin 3.xx` also update pyproject.toml and uv.lock?"
type: issue
state: open
author: adityashukzy
labels:
  - question
assignees: []
created_at: 2025-07-23T11:54:44Z
updated_at: 2025-08-14T23:18:48Z
url: https://github.com/astral-sh/uv/issues/14841
synced_at: 2026-01-07T13:12:19-06:00
---

# Does `uv python pin 3.xx` also update pyproject.toml and uv.lock?

---

_Issue opened by @adityashukzy on 2025-07-23 11:54_

### Question

Hello, I'm relatively new to uv. 

I cloned a git repo of mine to begin with, and then `uv init`'ed within the folder. This initialized the usual files (pyproject.toml, main.py, uv.lock, etc.).

However, at this point, the Python version picked up in .python-version was the default one in my `usr/bin/python` which was Python 3.9.

I wanted instead for it to install and use Python 3.10 for this project.

Thus, I ran the following:

1. `uv python install 3.10`
2. `uv python pin 3.10`

Now, as expected this change is reflected in the .python-version file (which displays 3.10). However, the pyproject.toml and uv.loc files still show the old info.

**pyproject.toml**:
```
requires-python = ">=3.9"
```

**uv.lock**:
```
version = 1
revision = 2
requires-python = ">=3.9"
```

Questions:
1. Do I manually need to change the values for requires-python in the above 2 files after doing `uv python pin 3.xx`? Is that even required? If so, why is that not taken care of by the pin command?
2. As I understand it, `uv python install 3.10` will install that Python version globally? Is there any way to install python only for this project / associated environment? I do not wish to pollute the global space with multiple Python versions.

As I am new, I may have some incorrect assumptions. So please correct me where needed.

uv has been a fantastic tool so far, so kudos to the community!

Thank you!

### Platform

macOS Sequoia 15.5 (Darwin 24.5.0 arm64)

### Version

uv 0.8.2 (21fadbcc1 2025-07-22)

---

_Label `question` added by @adityashukzy on 2025-07-23 11:54_

---

_Comment by @zanieb on 2025-07-23 12:03_

1. Yeah. The pin command is intended to change your default version. Since it's compatible with your `requires-python` range, there's nothing that _needs_ to change in the `pyproject.toml`. The `uv.lock` includes all possible Python versions already and doesn't need to change.
2. What is the concern about polluting the global namespace? i.e., where do you not want that Python to be available?

---

_Comment by @adityashukzy on 2025-08-14 23:18_

Okay that makes sense!

As to the latter question, I would imagine that multiple Python installations across the global namespace could lead to package / dependency conflicts, and this is the reason behind the typical advice to contain Python libraries (incl. the Python version itself) inside neat environments.

Am I mistaken in this understanding?

PS: my apologies for the late response - Github did not notify me for some reason. Thank you!

---
