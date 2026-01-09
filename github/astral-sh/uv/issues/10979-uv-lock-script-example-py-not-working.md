---
number: 10979
title: uv lock --script example.py not working
type: issue
state: closed
author: Kugeleis
labels:
  - bug
assignees: []
created_at: 2025-01-27T08:40:31Z
updated_at: 2025-01-30T20:50:00Z
url: https://github.com/astral-sh/uv/issues/10979
synced_at: 2026-01-07T13:12:18-06:00
---

# uv lock --script example.py not working

---

_Issue opened by @Kugeleis on 2025-01-27 08:40_

### Summary

I have a python project with various scripts I'd like to call from command line. The project is managed with uv and has a working venv in project/.venv. Calling a script if I'm in the folder with `uv run my_script.py` works fine.  

Calling the script form somewhere else with `uv run /full/path/to/my/project/my_script.py` fails with missing dependencies. 

I expected the following to cater for this.
According to[ the docs of uv version 0.5.24](https://docs.astral.sh/uv/guides/scripts/#locking-dependencies) `uv lock --script example.py` should create a lock file `example.py.lock`. It does not. The command line informs with _Resolved 103 packages in 3ms_ and returns without error.

### Platform

Windows 11

### Version

0.5.24

### Python version

3.12.8

---

_Label `bug` added by @Kugeleis on 2025-01-27 08:40_

---

_Comment by @charliermarsh on 2025-01-27 14:08_

Sorry, I can't reproduce this with a basic `uv init --script example.py` and `uv lock --script example.py` (and the issue itself doesn't include sufficient instructions for a real reproduction of whatever you're doing). I would need more information to help you, like the exact `example.py` that's failing, the series of commands you're running, and the verbose logs.

---

_Label `bug` removed by @charliermarsh on 2025-01-27 14:08_

---

_Label `needs-mre` added by @charliermarsh on 2025-01-27 14:08_

---

_Comment by @Kugeleis on 2025-01-28 08:43_

@charliermarsh I edited the explanation: I hope it makes the case clearer.

---

_Comment by @charliermarsh on 2025-01-29 16:11_

Can you please include the script you're using? Does it have a PEP 723 metadata tag?

---

_Comment by @Kugeleis on 2025-01-30 07:43_

@charliermarsh No, it does not come with inline dependency declaration. I made a [minimal example](https://github.com/Kugeleis/hello-uv). Running `uv lock --script hello.py` does nothing. Form somewhere else I can call it with `uv --project /long/path/to/my/project /long/path/to/my/project/hello.py` but this is overly verbose. I should be `uv --take-the-project-you-live-in /long/path/to/my/project/hello.py`.

---

_Comment by @charliermarsh on 2025-01-30 14:03_

It's because `hello.py` isn't a PEP 723 script. You should run `uv init --script hello.py` first, which gives you:

```python
# /// script
# requires-python = ">=3.13"
# dependencies = []
# ///

import numpy as np
import pandas as pd


def main() -> None:
    print("Hello from hello-uv!")
    df = pd.DataFrame(np.random.rand(3, 3))
    print(df)


if __name__ == "__main__":
    main()
```

But we should probably be erroring here or automatically initializing the script.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-30 14:03_

---

_Label `needs-mre` removed by @charliermarsh on 2025-01-30 14:03_

---

_Label `bug` added by @charliermarsh on 2025-01-30 14:03_

---

_Referenced in [astral-sh/uv#11118](../../astral-sh/uv/pulls/11118.md) on 2025-01-30 20:41_

---

_Closed by @charliermarsh on 2025-01-30 20:50_

---
