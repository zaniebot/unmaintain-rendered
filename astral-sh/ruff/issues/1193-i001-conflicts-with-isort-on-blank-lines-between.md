```yaml
number: 1193
title: I001 conflicts with isort on blank lines between import and comment before function
type: issue
state: closed
author: andersk
labels:
  - bug
  - isort
assignees: []
created_at: 2022-12-11T09:00:54Z
updated_at: 2022-12-11T22:13:11Z
url: https://github.com/astral-sh/ruff/issues/1193
synced_at: 2026-01-10T12:06:18Z
```

# I001 conflicts with isort on blank lines between import and comment before function

---

_Issue opened by @andersk on 2022-12-11 09:00_

As of ee994e8c074def26af7a9c3b43cf3e24e0238e89 (#1078), when an import is followed by a comment and then a function, Ruff `I001` wants two blank lines between the import and the comment, but isort wants one.

```console
$ cat test.py
import os

# comment


def f():
    pass
$ ruff --select=I001 --fix test.py
Found 1 error(s) (1 fixed, 0 remaining).
$ cat test.py
import os


# comment


def f():
    pass
$ isort --profile=black test.py
Fixing /tmp/test.py
$ cat test.py
import os

# comment


def f():
    pass
```

---

_Renamed from "I001 conflicts with isort on newline between import and comment before function" to "I001 conflicts with isort on blank lines between import and comment before function" by @andersk on 2022-12-11 09:01_

---

_Label `bug` added by @charliermarsh on 2022-12-11 14:08_

---

_Label `isort` added by @charliermarsh on 2022-12-11 14:08_

---

_Comment by @charliermarsh on 2022-12-11 14:12_

Hah, yeah, I was aware of this one but hadn’t prioritized fixing it since it’s kind of awkward in the current model. But I can. Did you run into it in the wild?

---

_Comment by @andersk on 2022-12-11 19:26_

I did:

[`zulip/tools/check-issue-labels`](https://github.com/zulip/zulip/blob/main/tools/check-issue-labels)
[`zulip/zerver/lib/ccache.py`](https://github.com/zulip/zulip/blob/main/zerver/lib/ccache.py)
[`zulip/zerver/lib/timeout.py`](https://github.com/zulip/zulip/blob/main/zerver/lib/timeout.py)
[`zulip/zerver/management/commands/send_to_email_mirror.py`](https://github.com/zulip/zulip/blob/main/zerver/management/commands/send_to_email_mirror.py)
[`zulip/zerver/migrations/0403_create_role_based_groups_for_internal_realms.py`](https://github.com/zulip/zulip/blob/main/zerver/migrations/0403_create_role_based_groups_for_internal_realms.py)
[`zulip/zerver/tests/test_migrations.py`](https://github.com/zulip/zulip/blob/main/zerver/tests/test_migrations.py)
[`zulip/zerver/webhooks/deskdotcom/tests.py`](https://github.com/zulip/zulip/blob/main/zerver/webhooks/deskdotcom/tests.py)


---

_Comment by @charliermarsh on 2022-12-11 20:24_

Okay cool, thanks for the examples! I’ll try to fix it today.

---

_Closed by @charliermarsh on 2022-12-11 22:13_

---
