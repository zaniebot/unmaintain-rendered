```yaml
number: 15296
title: "How to run `pyright` on a uv script"
type: issue
state: closed
author: stevenj
labels:
  - question
assignees: []
created_at: 2025-08-15T04:06:50Z
updated_at: 2025-11-16T20:00:28Z
url: https://github.com/astral-sh/uv/issues/15296
synced_at: 2026-01-12T16:02:07Z
```

# How to run `pyright` on a uv script

---

_@stevenj_

### Question

I have a uv script.
I want to run pyright on it.
Is it possible to run pyright on the script, inside the venv the script sets up when its run, so that pyright can properly check type annotations of the libraries the script uses?

I want to do this in CI, so i would rather not actually `run` the script itself.

I couldn't work out a way to achieve this.

### Platform

Any

### Version

0.8.8

---

_Label `question` added by @stevenj on 2025-08-15 04:06_

---

_Comment by @blueraft on 2025-08-15 08:08_

Probably [this](https://github.com/astral-sh/uv/pull/12763), but it's not merged yet.

Related [issue](https://github.com/astral-sh/uv/issues/6542)

---

_Comment by @ranma42 on 2025-11-16 09:38_

I have stumbled upon the same issue. In most cases, the following command seems to work:
```sh
uvx --with-requirements script.py pyright script.py
```
but I can't get it to work when the script uses alternative package indexes.

The example from https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies works:
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

```
$ uvx --with-requirements example.py pyright example.py
0 errors, 0 warnings, 0 informations
```

but this other script does not:

```python
# /// script
# dependencies = [
#     "torch==2.7.1+cpu",
# ]
#
# [[tool.uv.index]]
# name = "pytorch-cpu"
# url = "https://download.pytorch.org/whl/cpu"
# explicit = true
#
# [tool.uv.sources]
# torch = { index = "pytorch-cpu" }
# torchvision = { index = "pytorch-cpu" }
# ///

print('Done')
```

```
$ uvx --with-requirements bad.py pyright bad.py
  × No solution found when resolving tool dependencies:
  ╰─▶ Because there is no version of torch==2.7.1+cpu and you require torch==2.7.1+cpu, we can conclude that your requirements are unsatisfiable.
```

Note that the "bad" script actually works as intended when used for running:
```
$ uv run --script bad.py
Installed 10 packages in 420ms
Done
```

I am using uv[x] version 0.9.9.

---

_Comment by @ranma42 on 2025-11-16 09:48_

related:
 - https://github.com/astral-sh/uv/issues/10381
 - https://github.com/astral-sh/uv/issues/7032

---

_Closed by @zanieb on 2025-11-16 16:35_

---

_Comment by @ranma42 on 2025-11-16 20:00_

@zanieb do you think it might be worth mentioning this pattern in the [scripts docs](https://docs.astral.sh/uv/guides/scripts/) or is it just a weird corner case?

Also, for the alternative package index, should I follow #7032 or open a new feature request?

---
