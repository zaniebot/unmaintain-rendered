```yaml
number: 5822
title: "Should `uv run foo/bar.py` automatically run in the `foo` package?"
type: issue
state: open
author: charliermarsh
labels:
  - needs-decision
assignees: []
created_at: 2024-08-06T18:09:32Z
updated_at: 2024-08-20T18:21:34Z
url: https://github.com/astral-sh/uv/issues/5822
synced_at: 2026-01-12T15:58:59Z
```

# Should `uv run foo/bar.py` automatically run in the `foo` package?

---

_@charliermarsh_

See: https://github.com/astral-sh/ruff/pull/12622#issuecomment-2271225573

---

_Label `needs-decision` added by @charliermarsh on 2024-08-06 18:09_

---

_Label `preview` added by @charliermarsh on 2024-08-06 18:09_

---

_Comment by @zanieb on 2024-08-06 18:19_

Eeek. I think probably not? But maybe we want to provide a hint for the correct invocation?

---

_Comment by @charliermarsh on 2024-08-07 02:51_

Hmm yeah. But only on error...? Probably hard to detect.

---

_Comment by @zanieb on 2024-08-07 02:58_

I feel like part of my motivation for saying no is that there's not a clear way to go the other way around, like if we do `-p foo` by default then how do you get back to running the script in the workspace instead?

An always-on hint does sound problematic too.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---
