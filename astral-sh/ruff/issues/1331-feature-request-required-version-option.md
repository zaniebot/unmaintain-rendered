---
number: 1331
title: "[feature-request] `required-version` option"
type: issue
state: closed
author: smackesey
labels:
  - configuration
assignees: []
created_at: 2022-12-22T11:30:36Z
updated_at: 2022-12-26T00:53:51Z
url: https://github.com/astral-sh/ruff/issues/1331
synced_at: 2026-01-10T01:22:39Z
---

# [feature-request] `required-version` option

---

_Issue opened by @smackesey on 2022-12-22 11:30_

Black has a nice little option called `required-version`:

```
  --required-version TEXT         Require a specific version of Black to be
                                  running (useful for unifying results across
                                  many environments e.g. with a pyproject.toml
                                  file). It can be either a major version
                                  number or an exact version.
```

This is really useful for scenarios where black has been bumped but not all developers have updated their environments. It would be good for ruff to have a similar option (settable in config file and CLI) to help keep teams in sync.

---

_Comment by @charliermarsh on 2022-12-22 13:26_

Cool, easy enough!

---

_Label `configuration` added by @charliermarsh on 2022-12-22 13:26_

---

_Comment by @charliermarsh on 2022-12-22 16:21_

(This can probably go out today.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-24 20:04_

---

_Referenced in [astral-sh/ruff#1376](../../astral-sh/ruff/pulls/1376.md) on 2022-12-26 00:34_

---

_Comment by @charliermarsh on 2022-12-26 00:35_

Ok, took me a bit to get to this :)

---

_Closed by @charliermarsh on 2022-12-26 00:53_

---
