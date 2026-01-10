---
number: 3653
title: Should .mako.py files be ignored?
type: issue
state: closed
author: JonathanPlasse
labels:
  - question
assignees: []
created_at: 2023-03-21T17:02:20Z
updated_at: 2023-03-21T23:17:05Z
url: https://github.com/astral-sh/ruff/issues/3653
synced_at: 2026-01-10T01:22:42Z
---

# Should .mako.py files be ignored?

---

_Issue opened by @JonathanPlasse on 2023-03-21 17:02_

[Mozilla](https://github.com/mozilla/gecko-dev/blob/137075514eddc08c68ff652e9899da82e8043574/pyproject.toml#L71-L72) ignore mako files.

---

_Label `question` added by @charliermarsh on 2023-03-21 23:16_

---

_Comment by @charliermarsh on 2023-03-21 23:17_

I think what Mozilla is doing there strikes me as the right outcome. I could imagine it being confusing that Ruff omits those by defaults. In some rare cases, users might even _want_ them to checked. But it should be straightforward to omit them as they've done so there.

---

_Closed by @charliermarsh on 2023-03-21 23:17_

---

_Referenced in [astral-sh/ruff#14124](../../astral-sh/ruff/issues/14124.md) on 2024-11-07 12:39_

---
