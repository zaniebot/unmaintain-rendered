---
number: 11792
title: Additional dependencies in IPython profile
type: issue
state: closed
author: hugolundin
labels:
  - question
assignees: []
created_at: 2025-02-26T07:21:25Z
updated_at: 2025-02-28T05:23:35Z
url: https://github.com/astral-sh/uv/issues/11792
synced_at: 2026-01-10T01:25:10Z
---

# Additional dependencies in IPython profile

---

_Issue opened by @hugolundin on 2025-02-26 07:21_

### Question

I am a heavy user of IPython and typically have a REPL running all the time. My `profile_default/startup/start.py` looks like this, so that I don't have to import anything manually on a new session: 

```python
import numpy as np
import matplotlib.pyplot as plt
```

Up until now I have manually taken care of dependencies and installed them globally with `pip`, but I would like to convert this to `uv`. My first attempt was to simply use a `--with` statement. This works but quickly gets cumbersome as more dependencies are added:

```shell
$ uvx --with numpy,matplotlib ipython
```

I could use `--with-requirements` and have a `requirements.txt` for my IPython:

```shell
$ uvx --with-requirements ~/Developer/system/ipython/requirements.txt ipython
```

My ideal solution would, however, be to declare all dependencies inside `start.py`:

```python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "numpy",
#     "matplotlib",
# ]
# ///

import numpy as np
import matplotlib.pyplot as plt
```

It does not seem to work and assume that is because IPython is the one who calls `start.py` and not `uv`. Is there another way to do this that I have not mentioned?

I suppose the same could come up if you invoke a script in a regular Python REPL that declares it dependencies using the inline syntax. Ideally, there would be a way to tell `uv` that the script has dependencies, but since this cannot be determined before script runtime, it seems impossible. 

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @hugolundin on 2025-02-26 07:21_

---

_Comment by @zanieb on 2025-02-26 15:35_

I think this could be solved by https://github.com/astral-sh/uv/issues/6542

---

_Comment by @hugolundin on 2025-02-28 05:23_

Yes, that seems to solve my problem. Not sure how I did not find that issue when searching. Thank you. Closing. 

---

_Closed by @hugolundin on 2025-02-28 05:23_

---
