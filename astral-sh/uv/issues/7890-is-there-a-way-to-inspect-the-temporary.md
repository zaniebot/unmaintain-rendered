```yaml
number: 7890
title: Is there a way to inspect the temporary environment created?
type: issue
state: closed
author: gitgithan
labels:
  - question
assignees: []
created_at: 2024-10-03T08:27:09Z
updated_at: 2024-10-21T21:56:40Z
url: https://github.com/astral-sh/uv/issues/7890
synced_at: 2026-01-12T15:59:17Z
```

# Is there a way to inspect the temporary environment created?

---

_@gitgithan_

Previously I reported in #7730 that `uv run test_uv/demo.py` works unexpectedly but `uv run --python 3.10 test_uv/demo.py` fails.

I found a verbose flag in `uv run` but it only showed the python resolution process, with no information on the temporary environment created or used.

**Are details on the temporary environment created available in any part of the cli?** 
If not could anyone point to the source code where envs are get or created? 
Are they in `~/.cache/uv/environments-v1`?

I wish to understand the env creation process triggered by requesting a non-default python like in `uv run --python 3.10 test_uv/demo.py` to be able to predict what libraries will be available or not.  

For issue #7730, `uv run test_uv/demo.py` worked not because `The first case may have succeeded as uv run will use the .venv in the current directory by default` as Charlie suggested. 
I did not have any virtual env in the same directory that contained `rich`.
It turns out it was because uv was using the pip associated with the default python. 

Here's how I discovered that:
1. `uv python find` --> `/usr/bin/python3`
2. `ll /usr/bin/python3` --> `/usr/bin/python3 -> python3.8`
3. `python3.8 -m pip list` --> the list contained `rich` and `requests`
4. `pip3.8 uninstall rich` to break `uv run test_uv/demo.py`, thus proving uv was using environment (**is this considered system level?**) associated with python3.8 and pip3.8

---
Alternatively to step 3, `python3.8 -m pip show pip` , then look at `Location: /home/hanqi/.local/lib/python3.8/site-packages` to find `rich` inside 

---

_Label `question` added by @charliermarsh on 2024-10-03 11:54_

---

_Comment by @charliermarsh on 2024-10-03 11:55_

In this case, does the script have a `# /// script` tag?

---

_Comment by @gitgithan on 2024-10-03 13:12_

No `# /// script`, it was the same script as #7730 
```
import requests
from rich.pretty import pprint
import sys

print(sys.version)

import importlib.metadata

rich_version = importlib.metadata.version("rich")
print("Rich Version: ", rich_version)

resp = requests.get("https://peps.python.org/api/peps.json")
data = resp.json()
pprint([(k, v["title"]) for k, v in data.items()][:10])
```

---

_Comment by @charliermarsh on 2024-10-03 13:14_

If you're in a project (i.e., a directory or parent directory with a `pyproject.toml`), `uv run` will by default run in a virtual environment located at `.venv` (relative to the project root).

If you're _not_, it will run in the discovered interpreter (no environment necessary), unless you pass `--with`, in which case, it will create an ephemeral environment in the cache: https://github.com/astral-sh/uv/blob/005bb235f03c96d00d0231f1e1a4dd4eca0d4c0b/crates/uv/src/commands/project/environment.rs#L19

---

_Closed by @zanieb on 2024-10-21 21:56_

---
