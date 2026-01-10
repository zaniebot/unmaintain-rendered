---
number: 8097
title: "Inconsistent inline dependency collection when passing scripts through stdin using `uv run -`"
type: issue
state: closed
author: MatthewCane
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-10-10T15:42:36Z
updated_at: 2024-10-11T08:26:47Z
url: https://github.com/astral-sh/uv/issues/8097
synced_at: 2026-01-10T01:24:24Z
---

# Inconsistent inline dependency collection when passing scripts through stdin using `uv run -`

---

_Issue opened by @MatthewCane on 2024-10-10 15:42_

**platform**: MacOS Sequoia 15.0.1
**uv version:**: 0.4.20 (Homebrew 2024-10-08)
**shell**: zsh 5.9 (arm64-apple-darwin24.0)

Apologies if this is a duplicate but I can't find any other issues mentioning this.

#6481 added support for passing scripts into `uv` using stdin which is great addition, but it seems to be inconsistent for me when it comes to inline dependencies. The [example script on this doc page](https://docs.astral.sh/uv/guides/scripts/#running-a-script-with-dependencies) usually works:

```bash
> cat example.py
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

> cat example.py | uv run -
[
│   ('1', 'PEP Purpose and Guidelines'),
│   ('2', 'Procedure for Adding New Modules'),
│   ('3', 'Guidelines for Handling Bug Reports'),
│   ('4', 'Deprecation of Standard Modules'),
│   ('5', 'Guidelines for Language Evolution'),
│   ('6', 'Bug Fix Releases'),
│   ('7', 'Style Guide for C Code'),
│   ('8', 'Style Guide for Python Code'),
│   ('9', 'Sample Plaintext PEP Template'),
│   ('10', 'Voting Guidelines')
]
```
But will occasionally stop working for no apparent reason (this time with debug messages):

```bash
> cat example.py | uv run -v -
DEBUG uv 0.4.20 (Homebrew 2024-10-08)
DEBUG No project found; searching for Python interpreter
DEBUG Searching for default Python interpreter in managed installations or system path
DEBUG Found `cpython-3.11.10-macos-aarch64-none` at `/Users/matthewcane/Documents/tmp/uv-inline/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.11.10 interpreter at: /Users/matthewcane/Documents/tmp/uv-inline/.venv/bin/python3
DEBUG Running `python -c`
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'requests'
DEBUG Command exited with code: 1
```
A script using Polars (or Pandas) which works when called normally has never worked for me when passed in through stdin:

```bash
> cat example2.py
# /// script
# requires-python = ">=3.11"
# dependencies = [
#     "polars",
# ]
# ///
import polars

print(polars.build_info())

> uv run example2.py
Reading inline script metadata from: example2.py
{'version': '1.9.0'}

> cat example2.py | uv run -
Traceback (most recent call last):
  File "<string>", line 7, in <module>
ModuleNotFoundError: No module named 'polars'
```

For all of these tests I have ensured I am not in a VENV or anything like that. Please let me know if there are any tests I can run to get to the bottom of this issue.

And as always, thank you for all your hard work!!!

---

_Comment by @charliermarsh on 2024-10-10 16:06_

This is such weird timing hah... I learned about this like ten minutes ago from https://github.com/astral-sh/uv/pull/6375#issuecomment-2405355844. I agree, this should work.

---

_Label `bug` added by @charliermarsh on 2024-10-10 16:06_

---

_Label `help wanted` added by @charliermarsh on 2024-10-10 16:06_

---

_Referenced in [astral-sh/uv#6375](../../astral-sh/uv/pulls/6375.md) on 2024-10-10 16:06_

---

_Comment by @zanieb on 2024-10-10 16:52_

Should we require a `--script` flag here or should this always read PEP 723 metadata?

---

_Comment by @charliermarsh on 2024-10-10 17:36_

Personally I think we should always treat it as a script if it has the metadata (and not require that flag).

---

_Referenced in [astral-sh/uv#8111](../../astral-sh/uv/pulls/8111.md) on 2024-10-10 21:58_

---

_Closed by @charliermarsh on 2024-10-10 22:35_

---

_Closed by @charliermarsh on 2024-10-10 22:35_

---

_Comment by @MatthewCane on 2024-10-11 08:26_

That was fast! Thank you!

---
