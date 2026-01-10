---
number: 11197
title: Suggestions on language server support when using inline script metadata
type: issue
state: open
author: SamEdwardes
labels:
  - question
assignees: []
created_at: 2025-02-03T21:46:56Z
updated_at: 2025-02-12T16:50:22Z
url: https://github.com/astral-sh/uv/issues/11197
synced_at: 2026-01-10T01:25:02Z
---

# Suggestions on language server support when using inline script metadata

---

_Issue opened by @SamEdwardes on 2025-02-03 21:46_

### Question

Uv inline metadata is one of my favourite uv features:  https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies. 

One challenge I have found is that you do not get good editor support for 3rd party libraries. For example, you will not get auto completion of class methods because the editor does not know where the virtual environment is, and what packages will be installed in it. For example:

```python
# /// script
# dependencies = [
#   "requests<3",
#   "rich",
# ]
# ///

import requests
from rich.pretty import pprint

resp = requests.get("https://peps.python.org/api/peps.json")
data = resp.json()
pprint([(k, v["title"]) for k, v in data.items()][:10])
``` 

Do you have any suggestions on how to configure your editor, or other suggestions? I mostly use VS Code, Neo Vim,and Positron. 

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @SamEdwardes on 2025-02-03 21:46_

---

_Comment by @zanieb on 2025-02-03 21:51_

Most of the existing discussion is over in https://github.com/astral-sh/uv/issues/6637 in which I shared a command to create the environment at https://github.com/astral-sh/uv/issues/6637#issuecomment-2584434822

Unfortunately you'll still need to toggle your active virtual environment. I'm not aware of any solutions in editors yet, but hopefully we can improve the editor integration in the future!

---

_Comment by @SamEdwardes on 2025-02-10 21:39_

Thank you for the suggestion. I think my question is a bit different than https://github.com/astral-sh/uv/issues/6637. I am trying to think how I can tell the IDE to use the ephemeral virtual environment that will be created to run the script.

Either way, your comment does help! In the future it would be awesome if there was some kind of deeper IDE integration. E.g. VS Code recognized that the script has inline metadata, and somehow activates the right venv, or at least makes it available from the list of interpreters to choose from.

---

_Comment by @zanieb on 2025-02-10 23:14_

In VS Code you'd use `Python: Select interpreter` and select the script virtual environment.

We're working on #11361 which will at least use a stable path for the script environments so you can create and select it easily.

---

_Comment by @dnwe on 2025-02-12 16:50_

@zanieb just need that —edit next, a’la https://github.com/astral-sh/uv/issues/6637#issuecomment-2589997123 😅

---
