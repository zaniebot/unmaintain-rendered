```yaml
number: 3980
title: "`--index-strategy` not behaving as described"
type: issue
state: closed
author: casellimarco
labels:
  - needs-mre
assignees: []
created_at: 2024-06-03T10:47:01Z
updated_at: 2024-07-29T01:13:21Z
url: https://github.com/astral-sh/uv/issues/3980
synced_at: 2026-01-12T15:58:47Z
```

# `--index-strategy` not behaving as described

---

_@casellimarco_

## Scenario
I want to install the package `PACKAGE`. The package is present both on the global pypi, the default index-url, and in another extra-index-url `EXTRA_INDEX_URL`, but the specific version `VERSION_ONLY_ON_PYPI` is present only on the global pypi.

Theoretically, the extra_index_url would not be needed for this particular package, but since this is part of dependencies installation it is installed with many others packages through a requirements.txt file and for some of them the `EXTRA_INDEX_URL` is required.

When I try to install it with the command

``` uv pip install PACKAGE==VERSION_ONLY_ON_PYPI --extra-index-url EXTRA_INDEX_URL --index-strategy unsafe-best-match  --verbose```

I get something like

```
...
DEBUG Solving with target Python version 3.10.12
DEBUG Adding direct dependency: PACKAGE==VERSION_ONLY_ON_PYPI
DEBUG Found stale response for: EXTRA_INDEX_URL/PACKAGE
DEBUG Sending revalidation request for: EXTRA_INDEX_URL/PACKAGE
DEBUG Found modified response for: EXTRA_INDEX_URL/PACKAGE
DEBUG Found fresh response for: EXTRA_INDEX_URL/PACKAGE
DEBUG Searching for a compatible version of PACKAGE (==VERSION_ONLY_ON_PYPI)
DEBUG No compatible version found for: PACKAGE
 × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of PACKAGE==VERSION_ONLY_ON_PYPI and you require PACKAGE==VERSION_ONLY_ON_PYPI, we can conclude that the requirements are unsatisfiable.
```

To my understanding `unsafe-best-match` should look for all the versions in all the index-url (so Pypi's one and EXTRA_INDEX_URL) but as soon it finds the package in EXTRA_INDEX_URL but not the correct version it bombs without looking further.

## Setup
Python 3.10.12, Ubuntu 22.04, UV 0.2.5

---

_Comment by @charliermarsh on 2024-06-03 13:26_

Unfortunately I can't really do much with this -- it does work as described for me. Here's a concrete example:

```
echo "anyio==4.4.0" | uv pip compile --extra-index-url https://test.pypi.org/simple - --index-strategy unsafe-best-match
```

(`anyio` only exists at 3.5.0 on the Test PyPI index.)

---

_Label `needs-mre` added by @charliermarsh on 2024-06-03 13:26_

---

_Closed by @charliermarsh on 2024-07-29 01:13_

---
