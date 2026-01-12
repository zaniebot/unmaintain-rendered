```yaml
number: 3990
title: Workflow confusion about upgrading packages with requirements.txt (migrating from pip-tools)
type: issue
state: closed
author: mariokostelac
labels:
  - question
assignees: []
created_at: 2024-06-03T16:10:52Z
updated_at: 2024-06-05T07:30:19Z
url: https://github.com/astral-sh/uv/issues/3990
synced_at: 2026-01-12T15:58:47Z
```

# Workflow confusion about upgrading packages with requirements.txt (migrating from pip-tools)

---

_@mariokostelac_

What is the expected workflow for upgrading a package?

I'm in the process of migrating pip-tools to uv workflows. Previously, I'd use `pip-compile requirements.in --upgrade-package boto3==x -o requirements.txt`, which would take` requirements.tx`t (as constraints) into account and make sure that boto3 is upgraded. If it can't be because some transient deps conflict, it would upgrade necessary transient packages to make boto3 upgradeable to the version I've defined.

Right now, I use `uv pip compile -c requirements.txt requirements.in -P boto3 -o requirements.txt`, but it does nothing (there is newer version of boto3). If I remove boto3 manually from requirements.txt, it also does nothing (probably because of some transitive deps).

When using compiled `requirements.txt` as constraints file, what is the expected workflow procedure?

To be clear, the reason I use previously complied requirements.txt is to make sure that I don't get unrelated packages upgraded if transitive deps are not pinned.

`Version: uv 0.2.4 (98652e395 2024-05-26)`

---

_Comment by @charliermarsh on 2024-06-03 16:43_

The intended workflow here is `uv pip compile requirements.in --upgrade-package boto3 -o requirements.txt`. If you're trying to upgrade `boto3` to a specific version, then I'd suggest adding a constraint file that contains `boto3==x`. It sounds like the feature you're looking for is #1964.


---

_Label `question` added by @charliermarsh on 2024-06-03 16:43_

---

_Comment by @mariokostelac on 2024-06-05 07:27_

I have no idea what I as doing, but this works for me. For some reason I've started passing constraints file explicitly, which didn't work. Omitting it picks up the right set of deps and upgrades the package to newest version.

---

_Closed by @mariokostelac on 2024-06-05 07:30_

---
