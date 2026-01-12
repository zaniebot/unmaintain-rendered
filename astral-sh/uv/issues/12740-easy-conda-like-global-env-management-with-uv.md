```yaml
number: 12740
title: Easy conda-like global env management with uv
type: issue
state: closed
author: marksverdhei
labels:
  - enhancement
assignees: []
created_at: 2025-04-08T09:13:03Z
updated_at: 2025-04-08T16:49:02Z
url: https://github.com/astral-sh/uv/issues/12740
synced_at: 2026-01-12T16:01:11Z
```

# Easy conda-like global env management with uv

---

_@marksverdhei_

### Summary

IMO uv should have, but lacks a user friendly way to globally manage venvs.
I believe it would be better with support of a conda-like interface in addition to the current one.
This is a different issue from #1703 , as it only refers to the uv cli ux. I'm willing to give my shot for a contribution if desired.


### Example

create venv as usual
```bash
$ uv venv
uv venv
Using CPython 3.12.3 interpreter at: /usr/bin/python3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```
Then you have lots of different venvs in random different directories. uv keeps links to each venv created. Clearly, the example output here is not exactly how I want it to be desplayed. It is just the ruff idea.
```
$ uv list venv
name: torch-cuda-flash, location=/home/<you>/ml/torch-cuda-flash/.venv, python=/usr/bin/python3
name: agents, ...
```

Then a shorthand for activating them, deactivating them, etc.
```bash
$ uv activate torch-cuda-flash
# Runs 
# source /home/<you>/ml/torch-cuda-flash/.venv/bin/activate
# Under the hood.
```

---

_Label `enhancement` added by @marksverdhei on 2025-04-08 09:13_

---

_Comment by @shauneccles on 2025-04-08 11:02_

Why are you activating venvs?

---

_Comment by @marksverdhei on 2025-04-08 12:40_

... to access a specific group of installed python modules? Not sure what you're actually asking about


---

_Comment by @charliermarsh on 2025-04-08 15:37_

You're looking for https://github.com/astral-sh/uv/issues/1495, which is the issue we're using to track that behavior.

---

_Closed by @charliermarsh on 2025-04-08 15:37_

---

_Comment by @marksverdhei on 2025-04-08 16:49_

How did I miss that! Ty


---
