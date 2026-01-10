```yaml
number: 2202
title: "Issues with `B009` and `B010` autofix"
type: issue
state: closed
author: WilliamJamieson
labels:
  - bug
assignees: []
created_at: 2023-01-26T17:05:44Z
updated_at: 2023-01-26T17:56:21Z
url: https://github.com/astral-sh/ruff/issues/2202
synced_at: 2026-01-10T11:09:45Z
```

# Issues with `B009` and `B010` autofix

---

_Issue opened by @WilliamJamieson on 2023-01-26 17:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

I found that running the autofix for `B009` and `B010` can break code in a very specific circumstance, specifically when it is applied to:

- https://github.com/asdf-format/asdf/blob/ae9b77cdc8d69f1f1fd136ff48c9a523a86c18af/asdf/types.py#L85
-  https://github.com/asdf-format/asdf/blob/ae9b77cdc8d69f1f1fd136ff48c9a523a86c18af/asdf/types.py#L149

Will deeply break my code.

Based on this discussion thread: https://github.com/astropy/astropy/pull/14275#discussion_r1072555924, I think the issue is in how python handles `__X` access meaning this issue is likely limited to only cases where the attribute being set or gotten is of the form `__X`.

I think the solution might be to avoid making an autofix when the fixed string in question has the prefix `__` but no suffix `__`. 

---

_Comment by @charliermarsh on 2023-01-26 17:19_

Oh interesting! Will fix now.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-26 17:19_

---

_Label `bug` added by @charliermarsh on 2023-01-26 17:19_

---

_Comment by @charliermarsh on 2023-01-26 17:20_

Thank you for flagging this.

---

_Comment by @charliermarsh on 2023-01-26 17:22_

Ah right, ok, the double underscore causes name mangling.

---

_Comment by @WilliamJamieson on 2023-01-26 17:24_

Thanks for such a fast response!

---

_Comment by @sciyoshi on 2023-01-26 17:25_

Oh sorry @charliermarsh - didn't see you were already fixing! 😅 

---

_Comment by @charliermarsh on 2023-01-26 17:28_

No prob! Thanks so much for hopping on it!

---

_Unassigned @charliermarsh by @charliermarsh on 2023-01-26 17:28_

---

_Closed by @charliermarsh on 2023-01-26 17:56_

---
