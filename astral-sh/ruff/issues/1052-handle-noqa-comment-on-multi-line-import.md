```yaml
number: 1052
title: handle noqa comment on multi line import
type: issue
state: closed
author: xrmx
labels: []
assignees: []
created_at: 2022-12-05T10:35:42Z
updated_at: 2022-12-30T22:50:53Z
url: https://github.com/astral-sh/ruff/issues/1052
synced_at: 2026-01-10T12:05:22Z
```

# handle noqa comment on multi line import

---

_Issue opened by @xrmx on 2022-12-05 10:35_

In the following example where  something is imported but not used:

```
from foo import (  # noqa
    bar,
    baz,
)
```
flake8 applies the noqa to each import line but ruff 0.0.158 instead warns with `F401` for each line.

---

_Label `enhancement` added by @charliermarsh on 2022-12-05 14:10_

---

_Comment by @charliermarsh on 2022-12-16 16:39_

I'd like to make some improvements to the `noqa` system, and those improvements should enable this or something like it.

(Flake8 supports this because it locates every individual unused member error on the `from foo import (` line, which has a few downsides: you don't get individual span highlighting for individual members, there's no way to ignore _one_ unused member but not others.)


---

_Comment by @pradyunsg on 2022-12-30 03:03_

FWIW, I hit this while looking at replacing pip's use of `flake8` with `ruff`. 😅

---

_Comment by @charliermarsh on 2022-12-30 03:15_

😭 Everyone wants this! I need to just bite the bullet and support it.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-30 04:05_

---

_Comment by @charliermarsh on 2022-12-30 04:05_

I will try to fix this tomorrow.

---

_Closed by @charliermarsh on 2022-12-30 16:16_

---

_Comment by @charliermarsh on 2022-12-30 20:42_

This should be live shortly. [v0.0.203](https://github.com/charliermarsh/ruff/releases/tag/v0.0.203) is building now. Would love to know if it works for you (or doesn't).

---

_Comment by @TylerYep on 2022-12-30 22:50_

Fixed for me, thanks!

---
