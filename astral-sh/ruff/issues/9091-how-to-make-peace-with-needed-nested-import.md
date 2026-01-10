```yaml
number: 9091
title: How to make peace with needed nested import statements for Ruff E402?
type: issue
state: closed
author: alanwilter
labels:
  - rule
assignees: []
created_at: 2023-12-11T09:47:04Z
updated_at: 2025-07-21T13:21:48Z
url: https://github.com/astral-sh/ruff/issues/9091
synced_at: 2026-01-10T11:09:51Z
```

# How to make peace with needed nested import statements for Ruff E402?

---

_Issue opened by @alanwilter on 2023-12-11 09:47_

I use this lines a lot in several projects.

```python
import matplotlib
import numpy as np

matplotlib.use("Agg")

from matplotlib import pyplot as plt
from matplotlib.backends.backend_pdf import PdfPages
```

There's no other way that I know to set the matplotlib backend. But by doing so this will trigger [E402](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/).

`isort` has some ways of handling that (see [here](https://pycqa.github.io/isort/docs/configuration/action_comments.html)).

Of course I can fill lots of `# noqa: E402` but this is so ugly.

Ideally, Ruff/isort should be smart enough to know these cases.

Also, perhaps, page [E402](https://docs.astral.sh/ruff/rules/module-import-not-at-top-of-file/) (and all others when reasonable) should show how to silence this warning if the user knows what's doing.

---

_Comment by @konstin on 2023-12-11 13:47_

Could you share more details why `matplotlib.use` fails if it's after the other imports? https://matplotlib.org/stable/api/matplotlib_configuration_api.html#matplotlib.use reads like it should be possible.

> isort has some ways of handling that (see [here](https://pycqa.github.io/isort/docs/configuration/action_comments.html)).
> 
> Of course I can fill lots of # noqa: E402 but this is so ugly.

isort action comments look similar to the `noqa` comments to me, could you give an example of how they are different?


---

_Comment by @alanwilter on 2023-12-11 14:27_

If I load `from matplotlib import pyplot as plt` *before* `matplotlib.use("Agg")` it will be in interactive mode no matter what I do after. (ok, I can use `matplotlib.pyplot.ioff()`), but it will be slower to start and, if running remotely, it will start X11 service on my mac.

Another solution would be:

```python
import matplotlib
import numpy as np

from matplotlib.backends.backend_pdf import PdfPages
from matplotlib import pyplot as plt # need to be after the line above, but Ruff isort doesn't let
```

---

_Comment by @charliermarsh on 2023-12-11 15:21_

I've run into this `matplotlib` pattern before. I think it's actually reasonable for us to allow `matplotlib.use` calls to intersperse imports... In practice, I suspect it will strictly reduce false positives, with no false negatives.

---

_Comment by @charliermarsh on 2023-12-11 15:21_

I'll PR it and see what the ecosystem checks say.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-11 15:23_

---

_Label `rule` added by @charliermarsh on 2023-12-11 15:23_

---

_Closed by @charliermarsh on 2023-12-11 17:06_

---

_Comment by @alanwilter on 2023-12-14 08:30_

You guys are amazing. Father Xmas won't forget you ;-)

---

_Comment by @district10 on 2025-07-21 10:49_

So what's the solution?

I have a similar problem, `matplotlib.use("Agg")` -> `load_dotenv()`.
I don't want to <*fill lots of `# noqa: E402`*> (this is so ugly).


---

_Comment by @ntBre on 2025-07-21 13:21_

Looking at the linked PR, `matplotlib.use` was added as a special case, so I don't think there's a similar fix for `load_dotenv`. We do support the mentioned [isort](https://pycqa.github.io/isort/docs/configuration/action_comments.html) action comments in [Ruff](https://docs.astral.sh/ruff/linter/#action-comments) now, so that's one option. Otherwise it might make sense to open a separate, more general issue for discussion.

---
