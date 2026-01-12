```yaml
number: 3018
title: Fix E402 false positive
type: pull_request
state: closed
author: carlosmiei
labels: []
assignees: []
base: main
head: e402-fp
created_at: 2023-02-18T18:11:57Z
updated_at: 2023-02-20T18:00:25Z
url: https://github.com/astral-sh/ruff/pull/3018
synced_at: 2026-01-12T15:55:12Z
```

# Fix E402 false positive

---

_@carlosmiei_

Hello guys :) This is my first contribution to this fantastic project, sorry if I'm missing something. (I tried to follow all the `Contributing` recommendations)
- fixes https://github.com/charliermarsh/ruff/issues/3010





---

_Comment by @JonathanPlasse on 2023-02-18 18:16_

You got a fix fast! :+1:

---

_Comment by @charliermarsh on 2023-02-19 13:51_

Thank you! Asked one question in #3010 that I'd like to resolve before proceeding :)

---

_Comment by @carlosmiei on 2023-02-19 21:36_

> Thank you! Asked one question in #3010 that I'd like to resolve before proceeding :)

Sure, let me know if anything :)

---

_Comment by @charliermarsh on 2023-02-20 18:00_

Ok, I think for now, we should leave the current behavior as-is. My read is that the intent of pydocstyle is to only skip _docstrings_, and a multi-line string included after an import isn't a docstring.

If pydocstyle allowed _this_, then I'd be more convinced:

```py
"""abc """
"""def """
import os
```

But they flag that as an error, and indeed Python doesn't treat the second string as a docstring.

Thank you so much for the PR but I'm going to forego for now. I hope you continue contributing, it'd be great to have you involved!

---

_Closed by @charliermarsh on 2023-02-20 18:00_

---
