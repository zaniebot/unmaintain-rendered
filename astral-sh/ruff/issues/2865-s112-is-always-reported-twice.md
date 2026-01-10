```yaml
number: 2865
title: S112 is always reported twice
type: issue
state: closed
author: spaceone
labels:
  - question
assignees: []
created_at: 2023-02-13T15:49:50Z
updated_at: 2023-02-15T17:08:36Z
url: https://github.com/astral-sh/ruff/issues/2865
synced_at: 2026-01-10T11:09:45Z
```

# S112 is always reported twice

---

_Issue opened by @spaceone on 2023-02-13 15:49_

S112 is always reported twice for the same code

```
foo.py:328:13: S112 `try`-`except`-`continue` detected, consider logging the exception                                 
    |
328 |               except Exception:
    |  _____________^
329 | |                 continue
    | |________________________^ S112
    |

foo.py:328:13: S112 `try`-`except`-`continue` detected, consider logging the exception                                 
    |
328 |               except Exception:
    |  _____________^
329 | |                 continue
    | |________________________^ S112

```

---

_Comment by @charliermarsh on 2023-02-13 17:41_

Can you include the entire snippet, and the command, and your configuration? Running `cargo run crates/ruff/resources/test/fixtures/flake8_bandit/S112.py --isolated --select S -n` definitely doesn't report it twice, so there's something else going on.

---

_Label `question` added by @charliermarsh on 2023-02-13 17:41_

---

_Comment by @spaceone on 2023-02-13 18:19_

oh, the reason was that the file occurred twice in the given arguments: `ruff --select S112 --show-source foo.py foo.py` 
Maybe this should give a warning.

---

_Comment by @charliermarsh on 2023-02-13 18:20_

Ooh funny. Yeah maybe.

---

_Comment by @charliermarsh on 2023-02-13 18:21_

We should probably just deduplicate.

---

_Closed by @charliermarsh on 2023-02-15 17:08_

---
