---
number: 9579
title: namespace-packages option stopped working in a recent update
type: issue
state: closed
author: gokay05
labels:
  - question
assignees: []
created_at: 2024-01-19T16:19:17Z
updated_at: 2024-01-22T04:30:03Z
url: https://github.com/astral-sh/ruff/issues/9579
synced_at: 2026-01-10T01:22:49Z
---

# namespace-packages option stopped working in a recent update

---

_Issue opened by @gokay05 on 2024-01-19 16:19_

When running Ruff with version 0.1.11, namespace-packages option excluded INP001 from being thrown. I recently updated Ruff to 0.1.13 and even though my ruff.toml hasnt changed, I am getting a bunch of INP001 from my test folders and such. I checked the website and I dont see anything change in terms of how the option works, so I think there might be a bug in the new version that effectively breaks namespace-packages.

---

_Comment by @charliermarsh on 2024-01-19 16:21_

Hmm, I don't think we changed anything there in this version range. Are you able to share an example, including the configuration? It's _possible_ that your old setup was working due to caching, since the INP rules have a bug related to caching (and upgrading your Ruff version would clear the cache).

---

_Label `question` added by @charliermarsh on 2024-01-19 16:21_

---

_Comment by @gokay05 on 2024-01-21 22:25_

Here is a small example that reproduces the issue: https://github.com/gokay05/namespace-test 

When I run `ruff check --config ruff.toml` on this repo, I get the errors about the two folders that are in namespace-packages. 

![image](https://github.com/astral-sh/ruff/assets/62908441/e2208caf-8769-44ca-8ce6-cff507a41173)


---

_Comment by @charliermarsh on 2024-01-21 23:23_

Thanks so much for putting together a repro! I'll take a look.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-21 23:24_

---

_Comment by @charliermarsh on 2024-01-21 23:26_

Hm, something about specifying the config explicitly?

```
namespace-test on  main
❯ ruff check . -n
namespace-test on  main
❯ ruff check . --config ruff.toml -n
namespacepackages/namespacepackages/folder2/file2.py:1:1: INP001 File `namespacepackages/namespacepackages/folder2/file2.py` is part of an implicit namespace package. Add an `__init__.py`.
nonpackage/nonpackagefile.py:1:1: INP001 File `nonpackage/nonpackagefile.py` is part of an implicit namespace package. Add an `__init__.py`.
Found 2 errors.
```

---

_Comment by @charliermarsh on 2024-01-22 00:00_

Ah sorry, this is indeed a bug, my fault. Thank you so much for filing!

---

_Referenced in [astral-sh/ruff#9603](../../astral-sh/ruff/pulls/9603.md) on 2024-01-22 00:00_

---

_Comment by @charliermarsh on 2024-01-22 00:01_

Fixed in https://github.com/astral-sh/ruff/pull/9603, will go out in the next release.

---

_Closed by @charliermarsh on 2024-01-22 00:10_

---

_Comment by @gokay05 on 2024-01-22 00:38_

Thanks so much!! 

---

_Comment by @charliermarsh on 2024-01-22 04:30_

Thank _you_, I will always fix when there's a clear repro!

---
